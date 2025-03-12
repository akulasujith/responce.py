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
test\Services\test_APIResponse.py .FF.                                                                             [ 67%]
test\Services\test_Auth.py ....                                                                                    [ 72%]
test\Services\test_CustomException.py ..                                                                           [ 75%]
test\Services\test_dboperations.py ...........                                                                     [ 90%]
test\Services\test_fileoperations.py ....                                                                          [ 95%]
test\Services\test_logoperations.py .                                                                              [ 97%] 
test\Services\test_parentparser.py ..                                                                              [100%]

======================================================= FAILURES ======================================================== 
________________________________ TestApirespHandler.test_get_response_with_channel_info _________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_get_response_with_channel_info>

    def test_get_response_with_channel_info(self):
        handler = ApirespHandler()
        mock_data = {"key1": "value1", "channelname": "test_channel", "channelurl": "test_url"}
        mock_resp = MockResponse(mock_data)
        handler.setResponse(mock_resp)
>       response = handler.getResponse()

test\Services\test_APIResponse.py:55:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
test\Services\test_APIResponse.py:28: in getResponse
    return jsonify(respdict)
venv\lib\site-packages\flask\json\__init__.py:342: in jsonify
    return current_app.json.response(*args, **kwargs)
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of application context.
E
E           This typically means that you attempted to use functionality that needed
E           the current application. To solve this, set up an application context
E           with app.app_context(). See the documentation for more information.

venv\lib\site-packages\werkzeug\local.py:519: RuntimeError
_______________________________ TestApirespHandler.test_get_response_without_channel_info _______________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_get_response_without_channel_info>

    def test_get_response_without_channel_info(self):
        handler = ApirespHandler()
        mock_data = {"key1": "value1", "key2": "value2"}
        mock_resp = MockResponse(mock_data)
        handler.setResponse(mock_resp)
>       response = handler.getResponse()

test\Services\test_APIResponse.py:47: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
test\Services\test_APIResponse.py:28: in getResponse
    return jsonify(respdict)
venv\lib\site-packages\flask\json\__init__.py:342: in jsonify
    return current_app.json.response(*args, **kwargs)
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of application context.
E
E           This typically means that you attempted to use functionality that needed
E           the current application. To solve this, set up an application context
E           with app.app_context(). See the documentation for more information.

venv\lib\site-packages\werkzeug\local.py:519: RuntimeError
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
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_get_response_with_channel_info - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_get_response_without_channel_info - RuntimeError: Working outside of application context.
======================================= 2 failed, 71 passed, 10 warnings in 5.35s ======================================= 
PS C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API>
