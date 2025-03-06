pytest --cov . test/ --cov-report html
================================================== test session starts ==================================================
platform win32 -- Python 3.9.13, pytest-7.2.0, pluggy-1.5.0
rootdir: C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API
plugins: Flask-Dance-3.2.0, cov-4.0.0
collected 82 items

test\test_APIHome.py .........                                                                                     [ 10%]
test\test_config.py .....................                                                                          [ 36%]
test\test_db.py .                                                                                                  [ 37%] 
test\test_globalvars.py .                                                                                          [ 39%]
test\Entities\test_Customentities.py ....                                                                          [ 43%]
test\Entities\test_dbormschemas.py .........                                                                       [ 54%]
test\Services\test_APIResponse.py FFFFF.FFF.FFF                                                                    [ 70%]
test\Services\test_Auth.py ....                                                                                    [ 75%]
test\Services\test_CustomException.py ..                                                                           [ 78%] 
test\Services\test_dboperations.py ...........                                                                     [ 91%]
test\Services\test_fileoperations.py ....                                                                          [ 96%]
test\Services\test_logoperations.py .                                                                              [ 97%] 
test\Services\test_parentparser.py ..                                                                              [100%]

======================================================= FAILURES ======================================================== 
____________________________________ TestApirespHandler.test_getResponse_empty_resp _____________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_empty_resp>

    def test_getResponse_empty_resp(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {}
        self.handler.setResponse(mock_resp)
>       response = self.handler.getResponse()

test\Services\test_APIResponse.py:52:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
___________________________________ TestApirespHandler.test_getResponse_invalid_json ____________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_invalid_json>

    def test_getResponse_invalid_json(self):
        class NonSerializable:
            pass

        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key": NonSerializable()}
        self.handler.setResponse(mock_resp)
        with self.assertRaises(TypeError):
>           self.handler.getResponse()

test\Services\test_APIResponse.py:76:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
____________________________________ TestApirespHandler.test_getResponse_nested_data ____________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_nested_data>

    def test_getResponse_nested_data(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {
            "key1": {"nested_key": "value"},
            "key2": [1, 2, {"nested_key": "value"}]
        }
        self.handler.setResponse(mock_resp)
>       response = self.handler.getResponse()

test\Services\test_APIResponse.py:85:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
______________________________________ TestApirespHandler.test_getResponse_no_dict ______________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_no_dict>

    def test_getResponse_no_dict(self):
        mock_resp = MagicMock()
        del mock_resp.__dict__  # Simulate a response without __dict__
        self.handler.setResponse(mock_resp)
        with self.assertRaises(AttributeError):
>           self.handler.getResponse()

test\Services\test_APIResponse.py:66:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
__________________________________ TestApirespHandler.test_getResponse_non_string_keys __________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_non_string_keys>

    def test_getResponse_non_string_keys(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {123: "value"}
        self.handler.setResponse(mock_resp)
>       response = self.handler.getResponse()

test\Services\test_APIResponse.py:94:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
____________________________________ TestApirespHandler.test_getResponse_none_values ____________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_none_values>

    def test_getResponse_none_values(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": None, "key2": "value"}
        self.handler.setResponse(mock_resp)
>       response = self.handler.getResponse()

test\Services\test_APIResponse.py:102:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
_________________________________ TestApirespHandler.test_getResponse_with_channel_info _________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_with_channel_info>

    def test_getResponse_with_channel_info(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {
            "key1": "value1",
            "key2": 123,
            "channelname": "test",
            "channelurl": "http://test",
            "list_data": [1, 2, 3],
            "bool_data": True
        }
        self.handler.setResponse(mock_resp)
>       response = self.handler.getResponse()

test\Services\test_APIResponse.py:30:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\APIResponse.py:27: in getResponse
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
_______________________________ TestApirespHandler.test_getResponse_without_channel_info ________________________________ 

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_getResponse_without_channel_info>

    def test_getResponse_without_channel_info(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {"key1": "value1", "key2": 123}
        self.handler.setResponse(mock_resp)
>       response = self.handler.getResponse()

test\Services\test_APIResponse.py:43:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
Services\APIResponse.py:27: in getResponse
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
___________________________________ TestApirespHandler.test_setResponse_invalid_input ___________________________________

self = <test.Services.test_APIResponse.TestApirespHandler testMethod=test_setResponse_invalid_input>

    def test_setResponse_invalid_input(self):
        with self.assertRaises(TypeError):
>           self.handler.setResponse("invalid_input")
E           AssertionError: TypeError not raised

test\Services\test_APIResponse.py:17: AssertionError
___________________________________________ TestApirespBase.test_getResponse ____________________________________________ 

self = <test.Services.test_APIResponse.TestApirespBase testMethod=test_getResponse>

    def test_getResponse(self):
        #base = apir.ApirespBase()
        with self.assertRaises(NotImplementedError):
>           base.getResponse()
E           NameError: name 'base' is not defined

test\Services\test_APIResponse.py:116: NameError
___________________________________________ TestApirespBase.test_setResponse ____________________________________________ 

self = <test.Services.test_APIResponse.TestApirespBase testMethod=test_setResponse>

    def test_setResponse(self):
        #base = apir.ApirespBase()
        with self.assertRaises(NotImplementedError):
>           base.setResponse()
E           NameError: name 'base' is not defined

test\Services\test_APIResponse.py:111: NameError
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
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_empty_resp - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_invalid_json - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_nested_data - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_no_dict - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_non_string_keys - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_none_values - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_with_channel_info - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_getResponse_without_channel_info - RuntimeError: Working outside of application context.
FAILED test/Services/test_APIResponse.py::TestApirespHandler::test_setResponse_invalid_input - AssertionError: TypeError not raised
FAILED test/Services/test_APIResponse.py::TestApirespBase::test_getResponse - NameError: name 'base' is not defined       
FAILED test/Services/test_APIResponse.py::TestApirespBase::test_setResponse - NameError: name 'base' is not defined       
====================================== 11 failed, 71 passed, 10 warnings in 6.16s 
