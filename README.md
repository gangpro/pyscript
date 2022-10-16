# [PyScript] README
> SKILL : `pyscript`
> <br>
> PyScript Docs : [PyScript Docs](https://docs.pyscript.net/latest/tutorials/index.html)



## 목차
1. [PyScript 시작해보기](#pyscript-시작해보기)
2. [PyScript 설치](#pyscript-설치)
3. [PyScript HTML 파일 만들어보기](#pyscript-html-파일-만들어보기)
4. [py-script tag](#py-script-tag)
5. [py-config tag](#py-config-tag)
6. [Local modules](#local-modules)



## PyScript 시작해보기
- index.html 파일 생성
- VSCode Live 체크 : http://localhost:5500/
```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />

    <title>REPL</title>

    <link rel="icon" type="image/png" href="favicon.png" />
    <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
    <script defer src="https://pyscript.net/latest/pyscript.js"></script>
  </head>

  <body>
    Hello world! <br>
    This is the current date and time, as computed by Python:
    <py-script>
from datetime import datetime
now = datetime.now()
now.strftime("%m/%d/%Y, %H:%M:%S")
    </py-script>
  </body>
</html>
```



## PyScript 설치
- 별도의 설치 파일이 필요하지 않으며, ``<head>...</head>`` 태그 안에 넣으면 된다.

```html
<link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
<script defer src="https://pyscript.net/latest/pyscript.js"></script>
```



## PyScript HTML 파일 만들어보기
- hello.html 파일 생성
- VSCode Live 체크 : http://localhost:5500/hello.html
```html
<!-- hello.html -->
<html>
  <head>
    <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
    <script defer src="https://pyscript.net/latest/pyscript.js"></script>
  </head>
  <body> <py-script> print('Hello, World!') </py-script> </body>
</html>
```

## py-script tag
- ``py-script`` 태그 안에서는 여러줄의 코드를 작성 할 수 있다.
- VSCode Live 체크 : http://localhost:5500/compute-pi.html
```html
<!-- compute-pi.html -->
<html>
  <head>
    <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
    <script defer src="https://pyscript.net/latest/pyscript.js"></script>
  </head>
  <body>
      <py-script>
        print("Let's compute π:")
        def compute_pi(n):
            pi = 2
            for i in range(1,n):
                pi *= 4 * i ** 2 / (4 * i ** 2 - 1)
            return pi

        pi = compute_pi(100000)
        s = f"π is approximately {pi:.3f}"
        print(s)
      </py-script>
  </body>
</html>
```

- ``pyscript.write()``메소드를 사용하여 ``id``에 접근할 수 있다.
- VSCode Live 체크 : http://localhost:5500/today.html
```html
<!-- today.html -->
<html>
    <head>
      <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
      <script defer src="https://pyscript.net/latest/pyscript.js"></script>
      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" crossorigin="anonymous">
    </head>

  <body>
    <b><p>Today is <u><label id='today'></label></u></p></b>
    <br>
    <div id="pi" class="alert alert-primary"></div>
    <py-script>
      import datetime as dt
      pyscript.write('today', dt.date.today().strftime('%A %B %d, %Y'))

      def compute_pi(n):
          pi = 2
          for i in range(1,n):
              pi *= 4 * i ** 2 / (4 * i ** 2 - 1)
          return pi

      pi = compute_pi(100000)
      pyscript.write('pi', f'π is approximately {pi:.3f}')
    </py-script>
  </body>
</html>
```



## py-config tag
- ``<py-config>`` 태그를 사용하여 PyScript 앱 설정이 가능하다.
- ``TOML`` 또는 ``JSON`` 형식으로 설정해야 한다.
- ``<py-config>``는 ``<body>...</body>`` 태그 안에 넣는다.
```toml
<py-config>
  autoclose_loader = true

  [[runtimes]]
  src = "https://cdn.jsdelivr.net/pyodide/v0.21.2/full/pyodide.js"
  name = "pyodide-0.21.2"
  lang = "python"
</py-config>
```

```json
<py-config type="json">
  {
    "autoclose_loader": true,
    "runtimes": [{
      "src": "https://cdn.jsdelivr.net/pyodide/v0.21.2/full/pyodide.js",
      "name": "pyodide-0.21.2",
      "lang": "python"
    }]
  }
</py-config>
```

### 예제 1 - TOML 파일 불러오기
``src`` 속성에는 "파일 경로"를 추가한다.
```html
<py-config src="./custom.toml"></py-config>
```
```toml
# custom.toml
autoclose_loader = true
[[runtimes]]
src = "https://cdn.jsdelivr.net/pyodide/v0.21.2/full/pyodide.js"
name = "pyodide-0.21.2"
lang = "python"
```

### 예제 2 - JSON 파일 불러오기
``src`` 속성에는 "파일 경로"를,  ``type`` 속성에는 "json"을 추가한다.
```html
<py-config type="json" src="./custom.json"></py-config>
```

```json
{
  "autoclose_loader": true,
  "runtimes": [{
    "src": "https://cdn.jsdelivr.net/pyodide/v0.21.2/full/pyodide.js",
    "name": "pyodide-0.21.2",
    "lang": "python"
  }]
}
```

### 예제 3 - inline 추가
```html
<py-config src="./custom.toml">
  paths = ["./utils.py"]
</py-config>
```

json 파일 
```html
<py-config type="json" src="./custom.json">
  {
    "paths": ["./utils.py"]
  }
</py-config>
```

### 당연한 얘기지만, TOML & JSON 2개를 병행해서 사용할 수 없다.

### 예제 4 - NumPy와 Matplotlib 패키지 사용
```html
<!-- plot.html -->
<html>
    <head>
      <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
      <script defer src="https://pyscript.net/latest/pyscript.js"></script>
    </head>

  <body>
    <h1>Let's plot random numbers</h1>
    <div id="plot"></div>
    <py-config type="json">
        {
          "packages": ["numpy", "matplotlib"]
        }
    </py-config>
    <py-script output="plot">
      import matplotlib.pyplot as plt
      import numpy as np
      x = np.random.randn(1000)
      y = np.random.randn(1000)
      fig, ax = plt.subplots()
      ax.scatter(x, y)
      fig
    </py-script>
  </body>
</html>
```

## Local modules
패키지 외에도 로컬 파이썬(``.py``) 모듈을 사용할 수 있다. 

```py
# data.py
import numpy as np
def make_x_and_y(n):
    x = np.random.randn(n)
    y = np.random.randn(n)
    return x, y
```

```html
<!-- data.html -->
<html>
    <head>
      <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
      <script defer src="https://pyscript.net/latest/pyscript.js"></script>
    </head>

  <body>
    <h1>Let's plot random numbers</h1>
    <div id="plot"></div>
    <py-config type="toml">
        packages = ["numpy", "matplotlib"]
        paths = ["./data.py"]
    </py-config>
    <py-script output="plot">
      import matplotlib.pyplot as plt
      from data import make_x_and_y
      x, y = make_x_and_y(n=1000)
      fig, ax = plt.subplots()
      ax.scatter(x, y)
      fig
    </py-script>
  </body>
</html>
```