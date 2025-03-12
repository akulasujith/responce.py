pytest --cov . test/ --cov-report html
================================================== test session starts ==================================================
platform win32 -- Python 3.9.13, pytest-7.2.0, pluggy-1.5.0
rootdir: C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API
plugins: Flask-Dance-3.2.0, cov-4.0.0
collected 73 items

test\test_APIHome.py .........                                                                                     [ 12%]
test\test_config.py .....................                                                                          [ 41%]
test\test_db.py .                                                                                                  [ 42%] 
test\test_globalvars.py .                                                                                          [ 43%] 
test\Entities\test_Customentities.py ....                                                                          [ 49%]
test\Entities\test_dbormschemas.py .........                                                                       [ 61%]
test\Services\test_APIResponse.py F...                                                                             [ 67%]
test\Services\test_Auth.py ....                                                                                    [ 72%] 
test\Services\test_CustomException.py ..                                                                           [ 75%]
test\Services\test_dboperations.py ...........                                                                     [ 90%]
test\Services\test_fileoperations.py ....                                                                          [ 95%]
test\Services\test_logoperations.py .                                                                              [ 97%] 
test\Services\test_parentparser.py ..                                                                              [100%]

======================================================= FAILURES ======================================================== 
___________________________________ TestApirespHandler.test_abstract_methods_of_base ____________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_abstract_methods_of_base>

    def test_abstract_methods_of_base(self):
        class DummyApirespBase(ApirespBase):
            def setResponse(self, resp=None):
                pass

            def getResponse(self):
                pass

        dummy_instance = DummyApirespBase()
        self.assertTrue(isinstance(dummy_instance,ApirespBase))
>       base = ApirespBase()
E       TypeError: Can't instantiate abstract class ApirespBase with abstract methods getResponse, setResponse

test\Services\test_APIResponse.py:77: TypeError
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

venv\lib\site-packages\flask_sqlalchemy\__init__.py:14
venv\lib\site-packages\flask_sqlalchemy\__init__.py:14
  C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API\venv\lib\site-packages\flask_sqlalchemy\__init__.py:14: DeprecationWarning: '_app_ctx_stack' is deprecated and will be removed in Flask 2.3.
    from flask import _app_ctx_stack, abort, current_app, request

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html

---------- coverage: platform win32, python 3.9.13-final-0 -----------
Coverage HTML written to dir htmlcov

================================================ short test summary info ================================================ 
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_abstract_methods_of_base - TypeError: Can't instantiate abstract class ApirespBase with abstract methods getResponse, setResponse
======================================= 1 failed, 72 passed, 10 warnings in 4.03s ======================================= 
PS C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API> 
