```

def job():

    p = subprocess.Popen("python test_demo.py", shell=True, stdout=subprocess.PIPE, stderr=subprocess.STDOUT)

    buffs = p.stdout.readlines()



    for buff in buffs:

        buff = buff.decode("utf-8")

        print(buff)

```
