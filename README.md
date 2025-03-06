pytest --cov . test/ --cov-report html                 
================================================== test session starts ==================================================
platform win32 -- Python 3.9.13, pytest-7.2.0, pluggy-1.5.0
rootdir: C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API
plugins: Flask-Dance-3.2.0, cov-4.0.0
collected 70 items / 2 errors

======================================================== ERRORS ========================================================= 
_________________________________________ ERROR collecting test/test_APIHome.py _________________________________________ 
ImportError while importing test module 'C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\test\test_APIHome.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
C:\Program Files\Python39\lib\importlib\__init__.py:127: in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
test\test_APIHome.py:8: in <module>
    from APIHome import mainapp
APIHome.py:13: in <module>
    from db import db
db.py:1: in <module>
    from flask_sqlalchemy import SQLAlchemy
venv\lib\site-packages\flask_sqlalchemy\__init__.py:14: in <module>
    from flask import _app_ctx_stack, abort, current_app, request
E   ImportError: cannot import name '_app_ctx_stack' from 'flask' (C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\flask\__init__.py)
___________________________________________ ERROR collecting test/test_db.py ____________________________________________ 
ImportError while importing test module 'C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\test\test_db.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
C:\Program Files\Python39\lib\importlib\__init__.py:127: in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
test\test_db.py:2: in <module>
    from db import db
db.py:1: in <module>
    from flask_sqlalchemy import SQLAlchemy
venv\lib\site-packages\flask_sqlalchemy\__init__.py:14: in <module>
    from flask import _app_ctx_stack, abort, current_app, request
E   ImportError: cannot import name '_app_ctx_stack' from 'flask' (C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\flask\__init__.py)
=================================================== warnings summary ==================================================== 
venv\lib\site-packages\pandas\compat\numpy\__init__.py:10
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\pandas\compat\numpy\__init__.py:10: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    _nlv = LooseVersion(_np_version)

venv\lib\site-packages\pandas\compat\numpy\__init__.py:11
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\pandas\compat\numpy\__init__.py:11: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    np_version_under1p17 = _nlv < LooseVersion("1.17")

venv\lib\site-packages\pandas\compat\numpy\__init__.py:12
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\pandas\compat\numpy\__init__.py:12: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    np_version_under1p18 = _nlv < LooseVersion("1.18")

venv\lib\site-packages\pandas\compat\numpy\__init__.py:13
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\pandas\compat\numpy\__init__.py:13: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    _np_version_under1p19 = _nlv < LooseVersion("1.19")

venv\lib\site-packages\pandas\compat\numpy\__init__.py:14
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\pandas\compat\numpy\__init__.py:14: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    _np_version_under1p20 = _nlv < LooseVersion("1.20")

venv\lib\site-packages\setuptools\_distutils\version.py:337
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\setuptools\_distutils\version.py:337: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    other = LooseVersion(other)

venv\lib\site-packages\pandas\compat\numpy\function.py:120
venv\lib\site-packages\pandas\compat\numpy\function.py:120
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\pandas\compat\numpy\function.py:120: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
    if LooseVersion(__version__) >= LooseVersion("1.17.0"):

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html

---------- coverage: platform win32, python 3.9.13-final-0 -----------
Coverage HTML written to dir htmlcov

================================================ short test summary info ================================================ 
ERROR test/test_APIHome.py
ERROR test/test_db.py
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 2 errors during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 
============================================= 8 warnings, 2 errors in 3.15s ============================================= 
