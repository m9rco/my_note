# paramiko的几个实践
---
### 连接池

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os.path
import paramiko
from eventlet import pools


class SSHPool(pools.Pool):
    """A simple eventlet pool to hold ssh connections."""

    def __init__(self, ip, login, password=None,port=22,conn_timeout=3,
                 privatekey='~/.ssh/id_rsa', qdprivatekey=None, *args, **kwargs):
        self.ip = ip
        self.port = port
        self.login = login
        self.password = password
        self.conn_timeout = conn_timeout if conn_timeout else None
        self.privatekey = privatekey
        self.qdprivatekey = qdprivatekey
        if 'missing_key_policy' in kwargs.keys():
            self.missing_key_policy = kwargs.pop('missing_key_policy')
        else:
            self.missing_key_policy = paramiko.AutoAddPolicy()
        if 'hosts_key_file' in kwargs.keys():
            self.hosts_key_file = kwargs.pop('hosts_key_file')
        else:
            self.hosts_key_file = None
        if 'transport_sock_timeout' in kwargs.keys():
            self.transport_sock_timeout = kwargs.pop('transport_sock_timeout')
        else:
            self.transport_sock_timeout = 0.1
        super(SSHPool, self).__init__(*args, **kwargs)

    def create(self):
        try:
            ssh = paramiko.SSHClient()
            ssh.set_missing_host_key_policy(self.missing_key_policy)
            if self.hosts_key_file:
                ssh.load_host_keys(self.hosts_key_file)
            if self.qdprivatekey:
                # 如果有qdprivatekey的话，就不用密码了
                ssh.connect(self.ip,
                            port=self.port,
                            username='root',
                            pkey=self.qdprivatekey,
                            timeout=self.conn_timeout)
            elif self.password:
                ssh.connect(self.ip,
                            port=self.port,
                            username=self.login,
                            password=self.password,
                            timeout=self.conn_timeout)
            elif self.privatekey:
                pkfile = os.path.expanduser(self.privatekey)
                privatekey = paramiko.RSAKey.from_private_key_file(pkfile)
                ssh.connect(self.ip,
                            port=self.port,
                            username=self.login,
                            pkey=privatekey,
                            timeout=self.conn_timeout)
            else:
                raise Exception("Specify a password or private_key")

            # Paramiko by default sets the socket timeout to 0.1 seconds,
            # ignoring what we set through the sshclient. This doesn't help for
            # keeping long lived connections. Hence we have to bypass it, by
            # overriding it after the transport is initialized. We are setting
            # the sockettimeout to None and setting a keepalive packet so that,
            # the server will keep the connection open. All that does is send
            # a keepalive packet every ssh_conn_timeout seconds.
            if self.conn_timeout:
                transport = ssh.get_transport()
                transport.sock.settimeout(self.transport_sock_timeout)
                transport.set_keepalive(self.conn_timeout)
            return ssh
        except paramiko.ssh_exception.AuthenticationException:
            raise AuthException
        except Exception as e:
            msg = "Error connecting via ssh: %s" % e
            raise paramiko.SSHException(msg)

    def get(self):
        """Return an item from the pool, when one is available.

        This may cause the calling greenthread to block. Check if a
        connection is active before returning it.

        For dead connections create and return a new connection.
        """
        conn = super(SSHPool, self).get()
        if conn:
            if conn.get_transport().is_active():
                return conn
            else:
                conn.close()
        return self.create()

    def get_ssh_channel(self):
        conn = self.get()
        transport = conn.get_transport()
        transport.set_keepalive(1)

        channel = transport.open_session()
        channel.get_pty()
        channel.invoke_shell()
        channel.settimeout(1.0)
        return conn, channel

    def remove(self, ssh):
        """Close an ssh client and remove it from free_items."""
        ssh.close()
        ssh = None
        if ssh in self.free_items:
            self.free_items.pop(ssh)
        if self.current_size > 0:
            self.current_size -= 1


class AuthException(paramiko.ssh_exception.SSHException):
    pass


class SSHException(paramiko.ssh_exception.SSHException):
    pass
```

### 利用paramiko的client执行命令
```
class Demo():
    def init_ssh_channel(self, qdprivatekey=None):
        """
        init ssh , get client, channel and transport
        :param qdprivatekey:
        :return:
        """
        self.sshPool = \
            sshutil.SSHPool(ip=self.db_args['hostip'], port=int(self.db_args['ssh_port']),
                            conn_timeout=global_info.ssh_time_out, login=self.db_args['os_user'],
                            password=self.db_args['os_user_pwd'], qdprivatekey=qdprivatekey)
        self.ssh = self.sshPool.get()
        self.channel = self.ssh.invoke_shell()
        self.transport = self.ssh.get_transport()
        if not self.transport.is_authenticated():
            raise Exception("Authentication failed")
    def release_ssh_channel(self):
        """
        释放ssh资源回到连接池
        """
        try:
            self.sshPool.remove(self.ssh)
        except:
            print_traceback()

    def _run_cmd(self, cmd):
        """
        run command in ssh client

        :example
            cmd: "pwd"
            result:
                {
                    "exit_code": 0,
                    "stdout": "/home/tt67wq/code",
                    "stderr": ""
                }
        """
        try:
            result = dict()
            stdin, stdout, stderr = self.ssh.exec_command(cmd)
            stdin.close()
            stdout.flush()
            out_msg = stdout.read()
            err_msg = stderr.read()
            result["stdout"] = out_msg
            result["stderr"] = err_msg
            result["exit_code"] = 0
            if err_msg:
                result["exit_code"] = 1
            return result
        except Exception as e:
            result = dict()
            result["stdout"] = ""
            result["stderr"] = e
            result["exit_code"] = 2
            return result

    def run_cmd(self, cmd):
        """
        use to parse the return buff from function `_run_cmd`
        :param result:
        :return:
        """
        result = self._run_cmd(cmd=cmd)
        if result.get("exit_code") == 0:
            return result.get("stdout").strip()
        else:
            raise Exception(result.get("stderr"))
```
