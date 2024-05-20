---
title: PyCharm 원격 개발에서 UnicodeDecodeError가 발생하는 문제
tags: JetBrains PyCharm
key: 202401031801
---

안녕하세요 삼각형입니다.

PyCharm 원격 개발에서 `UnicodeDecodeError: 'utf-8' codec can't decode byte 0x80 in position 64: invalid start byte`가 발생하는 경우가 있습니다. 이를 해결하는 방법은 다음과 같습니다.

1. 에러 메세지에서 `site.py`의 경로를 확인합니다.
   
   ```
   Fatal Python error: init_import_size: Failed to import the site module
   Python runtime state: initialized
   Traceback (most recent call last):
     File ".../site.py", line 73, in <module>
       __boot()
     File ".../site.py", line 47, in __boot
       addsitedir(item)
     File "/usr/lib/python3.8/site.py", line 214, in addsitedir
       addpackage(sitedir, name, known_paths)
     File "/usr/lib/python3.8/site.py", line 170, in addpackage
       for n, line in enumerate(f):
     File "/usr/lib/python3.8/codecs.py", line 322, in decode
       (result, consumed) = self._buffer_decode(data, self.errors, final)
   UnicodeDecodeError: 'utf-8' codec can't decode byte 0x80 in position 64: invalid start byte
   ```

2. `site.py`를 `site.py.bak`으로 변경합니다.
   
   ```
   mv .../site.py .../site.py.bak
   ```

감사합니다.

<!--more-->
