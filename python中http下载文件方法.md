# 文件下载方法

标签（空格分隔）： Python

---

```
def get(self, **kwargs):
        # print self.current_user
        # self.render("index.html")
        import os
        _file_dir = os.path.abspath("/tmp")
        file_name = "test2.py"
        _file_path = os.path.join(_file_dir, file_name)
        if not os.path.exists(_file_path):
            self.write("error")
            self.finish()
        if not file_name or not os.path.exists(_file_path):
            raise HTTPError(404)
        self.set_header('Content-Type', 'application/force-download')
        self.set_header('Content-Disposition', 'attachment; filename=%s' % file_name)
        with open(_file_path, "rb") as f:
            try:
                while True:
                    _buffer = f.read(4096)
                    if _buffer:
                        self.write(_buffer)
                    else:
                        f.close()
                        self.finish()
                        return
            except:
                self.write("error")
                # raise HTTPError(404)
```
