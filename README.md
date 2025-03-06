pytest --cov . test/ --cov-report html
================================================== test session starts ==================================================
platform win32 -- Python 3.9.13, pytest-7.2.0, pluggy-1.5.0
rootdir: C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API
plugins: Flask-Dance-3.2.0, cov-4.0.0
collected 80 items

test\test_APIHome.py .........                                                                                     [ 11%]
test\test_config.py .....................                                                                          [ 37%]
test\test_db.py .                                                                                                  [ 38%] 
test\test_globalvars.py .                                                                                          [ 40%] 
test\Entities\test_Customentities.py ....                                                                          [ 45%]
test\Entities\test_dbormschemas.py .........                                                                       [ 56%]
test\Services\test_APIResponse.py ...F....FFF                                                                      [ 70%]
test\Services\test_Auth.py ....                                                                                    [ 75%]
test\Services\test_CustomException.py ..                                                                           [ 77%]
test\Services\test_dboperations.py ...........                                                                     [ 91%]
test\Services\test_fileoperations.py ....                                                                          [ 96%]
test\Services\test_logoperations.py .                                                                              [ 97%] 
test\Services\test_parentparser.py ..                                                                              [100%]

======================================================= FAILURES ======================================================== 
______________________________________ TestApirespHandler.test_getResponse_no_dict ______________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_no_dict>

    def test_getResponse_no_dict(self):
        mock_resp = MagicMock()
        del mock_resp.__dict__
        self.handler.setResponse(mock_resp)
        with self.assertRaises(AttributeError):
>           self.handler.getResponse()

test\Services\test_APIResponse.py:50:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
    return jsonify(respdict)
venv\lib\site-packages\flask\json\__init__.py:342: in jsonify
    return current_app.json.response(*args, **kwargs)
venv\lib\site-packages\flask\json\provider.py:309: in response
    f"{self.dumps(obj, **dump_args)}\n", mimetype=mimetype
venv\lib\site-packages\flask\json\provider.py:230: in dumps
    return json.dumps(obj, **kwargs)
C:\Program Files\Python39\lib\json\__init__.py:234: in dumps
    return cls(
C:\Program Files\Python39\lib\json\encoder.py:199: in encode
    chunks = self.iterencode(o, _one_shot=True)
C:\Program Files\Python39\lib\json\encoder.py:257: in iterencode
    return _iterencode(o, 0)
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _default(o: t.Any) -> t.Any:
        if isinstance(o, date):
            return http_date(o)

        if isinstance(o, (decimal.Decimal, uuid.UUID)):
            return str(o)

        if dataclasses and dataclasses.is_dataclass(o):
            return dataclasses.asdict(o)

        if hasattr(o, "__html__"):
            return str(o.__html__())

>       raise TypeError(f"Object of type {type(o).__name__} is not JSON serializable")
E       TypeError: Object of type _SentinelObject is not JSON serializable

venv\lib\site-packages\flask\json\provider.py:122: TypeError
___________________________________ TestApirespHandler.test_setResponse_invalid_input ___________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_setResponse_invalid_input>

    def test_setResponse_invalid_input(self):
        with self.assertRaises(TypeError):
>           self.handler.setResponse("invalid_input")
E           AssertionError: TypeError not raised

test\Services\test_APIResponse.py:89: AssertionError
___________________________________________ TestApirespBase.test_getResponse ____________________________________________ 

self = <test.Services.test_APIResponse.TestApirespBase testMethod=test_getResponse>

    def setUp(self):
>       self.base = apir.ApirespBase() #instantiate ApirespBase
E       TypeError: Can't instantiate abstract class ApirespBase with abstract methods getResponse, setResponse

test\Services\test_APIResponse.py:93: TypeError
___________________________________________ TestApirespBase.test_setResponse ____________________________________________ 

self = <test.Services.test_APIResponse.TestApirespBase testMethod=test_setResponse>

    def setUp(self):
>       self.base = apir.ApirespBase() #instantiate ApirespBase
E       TypeError: Can't instantiate abstract class ApirespBase with abstract methods getResponse, setResponse

test\Services\test_APIResponse.py:93: TypeError
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
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_no_dict - TypeError: Object of type _SentinelObject is not JSON serializable
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_setResponse_invalid_input - AssertionError: TypeError not raised
FAILED test/Services/test_APIResponse.py::TestApirespBase::test_getResponse - TypeError: Can't instantiate abstract class ApirespBase with abstract methods getResponse, setResponse
FAILED test/Services/test_APIResponse.py::TestApirespBase::test_setResponse - TypeError: Can't instantiate abstract class ApirespBase with abstract methods getResponse, setResponse
======================================= 4 failed, 76 passed, 10 warnings in 4.27s ==================
