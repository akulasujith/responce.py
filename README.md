pytest --cov . test/ --cov-report html
================================================== test session starts ==================================================
platform win32 -- Python 3.9.13, pytest-7.2.0, pluggy-1.5.0
rootdir: C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API
plugins: Flask-Dance-3.2.0, cov-4.0.0
collected 77 items

test\test_APIHome.py .........                                                                                     [ 11%]
test\test_config.py .....................                                                                          [ 38%]
test\test_db.py .                                                                                                  [ 40%] 
test\test_globalvars.py .                                                                                          [ 41%] 
test\Entities\test_Customentities.py ....                                                                          [ 46%]
test\Entities\test_dbormschemas.py .........                                                                       [ 58%]
test\Services\test_APIResponse.py ....                                                                             [ 63%]
test\Services\test_Auth.py .FFFFFFF                                                                                [ 74%]
test\Services\test_CustomException.py ..                                                                           [ 76%] 
test\Services\test_dboperations.py ...........                                                                     [ 90%]
test\Services\test_fileoperations.py ....                                                                          [ 96%]
test\Services\test_logoperations.py .                                                                              [ 97%] 
test\Services\test_parentparser.py ..                                                                              [100%]

======================================================= FAILURES ======================================================== 
_______________________________________ TestAuth.test_GetLoggedInUser_with_domain _______________________________________ 

self = <test.Services.test_Auth.TestAuth testMethod=test_GetLoggedInUser_with_domain>

    def test_GetLoggedInUser_with_domain(self):
        token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6InRlc3RAU01GQ0dELkNPTSJ9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
        GetLoggedInUser(token)
>       self.assertEqual(gvar.user_id, "test")
E       AssertionError: 'test@SMFCGD.COM' != 'test'
E       - test@SMFCGD.COM
E       + test

test\Services\test_Auth.py:37: AssertionError
_____________________________________ TestAuth.test_GetLoggedInUser_without_domain ______________________________________ 

self = <jwt.api_jws.PyJWS object at 0x000001B0E732BD60>
jwt = b'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9eyJ1bmlxdWVfbmFtZSI6InRlc3QifQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c'   

    def _load(self, jwt):
        if isinstance(jwt, str):
            jwt = jwt.encode("utf-8")

        if not isinstance(jwt, bytes):
            raise DecodeError(f"Invalid token type. Token must be a {bytes}")

        try:
            signing_input, crypto_segment = jwt.rsplit(b".", 1)
>           header_segment, payload_segment = signing_input.split(b".", 1)
E           ValueError: not enough values to unpack (expected 2, got 1)

venv\lib\site-packages\jwt\api_jws.py:221: ValueError

The above exception was the direct cause of the following exception:

self = <test.Services.test_Auth.TestAuth testMethod=test_GetLoggedInUser_without_domain>

    def test_GetLoggedInUser_without_domain(self):
        token = "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9eyJ1bmlxdWVfbmFtZSI6InRlc3QifQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
>       GetLoggedInUser(token)

test\Services\test_Auth.py:41:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
Services\Auth.py:78: in GetLoggedInUser
    decrypted_token = DecryptToken(token)
Services\Auth.py:22: in DecryptToken
    decoded_token = jwt.decode(data, options={"verify_signature": False})
venv\lib\site-packages\jwt\api_jwt.py:129: in decode
    decoded = self.decode_complete(jwt, key, algorithms, options, **kwargs)
venv\lib\site-packages\jwt\api_jwt.py:100: in decode_complete
    decoded = api_jws.decode_complete(
venv\lib\site-packages\jwt\api_jws.py:171: in decode_complete
    payload, signing_input, header, signature = self._load(jwt)
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

self = <jwt.api_jws.PyJWS object at 0x000001B0E732BD60>
jwt = b'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9eyJ1bmlxdWVfbmFtZSI6InRlc3QifQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c'   

    def _load(self, jwt):
        if isinstance(jwt, str):
            jwt = jwt.encode("utf-8")

        if not isinstance(jwt, bytes):
            raise DecodeError(f"Invalid token type. Token must be a {bytes}")

        try:
            signing_input, crypto_segment = jwt.rsplit(b".", 1)
            header_segment, payload_segment = signing_input.split(b".", 1)
        except ValueError as err:
>           raise DecodeError("Not enough segments") from err
E           jwt.exceptions.DecodeError: Not enough segments

venv\lib\site-packages\jwt\api_jws.py:223: DecodeError
______________________________________ TestAuth.test_token_required_invalid_token _______________________________________ 
C:\Program Files\Python39\lib\unittest\mock.py:1333: in patched
    with self.decoration_helper(patched,
C:\Program Files\Python39\lib\contextlib.py:119: in __enter__
    return next(self.gen)
C:\Program Files\Python39\lib\unittest\mock.py:1315: in decoration_helper
    arg = exit_stack.enter_context(patching)
C:\Program Files\Python39\lib\contextlib.py:448: in enter_context
    result = _cm_type.__enter__(cm)
C:\Program Files\Python39\lib\unittest\mock.py:1427: in __enter__
    if spec is None and _is_async_obj(original):
C:\Program Files\Python39\lib\unittest\mock.py:50: in _is_async_obj
    if hasattr(obj, '__func__'):
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of request context.
E
E           This typically means that you attempted to use functionality that needed
E           an active HTTP request. Consult the documentation on testing for
E           information about how to avoid this problem.

venv\lib\site-packages\werkzeug\local.py:519: RuntimeError
______________________________________ TestAuth.test_token_required_missing_token _______________________________________ 
C:\Program Files\Python39\lib\unittest\mock.py:1333: in patched
    with self.decoration_helper(patched,
C:\Program Files\Python39\lib\contextlib.py:119: in __enter__
    return next(self.gen)
C:\Program Files\Python39\lib\unittest\mock.py:1315: in decoration_helper
    arg = exit_stack.enter_context(patching)
C:\Program Files\Python39\lib\contextlib.py:448: in enter_context
    result = _cm_type.__enter__(cm)
C:\Program Files\Python39\lib\unittest\mock.py:1427: in __enter__
    if spec is None and _is_async_obj(original):
C:\Program Files\Python39\lib\unittest\mock.py:50: in _is_async_obj
    if hasattr(obj, '__func__'):
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of request context.
E
E           This typically means that you attempted to use functionality that needed
E           an active HTTP request. Consult the documentation on testing for
E           information about how to avoid this problem.

venv\lib\site-packages\werkzeug\local.py:519: RuntimeError
__________________________________ TestAuth.test_token_required_valid_token_authorized __________________________________ 
C:\Program Files\Python39\lib\unittest\mock.py:1333: in patched
    with self.decoration_helper(patched,
C:\Program Files\Python39\lib\contextlib.py:119: in __enter__
    return next(self.gen)
C:\Program Files\Python39\lib\unittest\mock.py:1315: in decoration_helper
    arg = exit_stack.enter_context(patching)
C:\Program Files\Python39\lib\contextlib.py:448: in enter_context
    result = _cm_type.__enter__(cm)
C:\Program Files\Python39\lib\unittest\mock.py:1427: in __enter__
    if spec is None and _is_async_obj(original):
C:\Program Files\Python39\lib\unittest\mock.py:50: in _is_async_obj
    if hasattr(obj, '__func__'):
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of request context.
E
E           This typically means that you attempted to use functionality that needed
E           an active HTTP request. Consult the documentation on testing for
E           information about how to avoid this problem.

venv\lib\site-packages\werkzeug\local.py:519: RuntimeError
_______________________________ TestAuth.test_token_required_valid_token_empty_user_list ________________________________ 
C:\Program Files\Python39\lib\unittest\mock.py:1333: in patched
    with self.decoration_helper(patched,
C:\Program Files\Python39\lib\contextlib.py:119: in __enter__
    return next(self.gen)
C:\Program Files\Python39\lib\unittest\mock.py:1315: in decoration_helper
    arg = exit_stack.enter_context(patching)
C:\Program Files\Python39\lib\contextlib.py:448: in enter_context
    result = _cm_type.__enter__(cm)
C:\Program Files\Python39\lib\unittest\mock.py:1427: in __enter__
    if spec is None and _is_async_obj(original):
C:\Program Files\Python39\lib\unittest\mock.py:50: in _is_async_obj
    if hasattr(obj, '__func__'):
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of request context.
E
E           This typically means that you attempted to use functionality that needed
E           an active HTTP request. Consult the documentation on testing for
E           information about how to avoid this problem.

venv\lib\site-packages\werkzeug\local.py:519: RuntimeError
_________________________________ TestAuth.test_token_required_valid_token_unauthorized _________________________________ 
C:\Program Files\Python39\lib\unittest\mock.py:1333: in patched
    with self.decoration_helper(patched,
C:\Program Files\Python39\lib\contextlib.py:119: in __enter__
    return next(self.gen)
C:\Program Files\Python39\lib\unittest\mock.py:1315: in decoration_helper
    arg = exit_stack.enter_context(patching)
C:\Program Files\Python39\lib\contextlib.py:448: in enter_context
    result = _cm_type.__enter__(cm)
C:\Program Files\Python39\lib\unittest\mock.py:1427: in __enter__
    if spec is None and _is_async_obj(original):
C:\Program Files\Python39\lib\unittest\mock.py:50: in _is_async_obj
    if hasattr(obj, '__func__'):
venv\lib\site-packages\werkzeug\local.py:318: in __get__
    obj = instance._get_current_object()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

    def _get_current_object() -> T:
        try:
            obj = local.get()
        except LookupError:
>           raise RuntimeError(unbound_message) from None
E           RuntimeError: Working outside of request context.
E
E           This typically means that you attempted to use functionality that needed
E           an active HTTP request. Consult the documentation on testing for
E           information about how to avoid this problem.

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
FAILED test/Services/test_Auth.py::TestAuth::test_GetLoggedInUser_with_domain - AssertionError: 'test@SMFCGD.COM' != 'test'
FAILED test/Services/test_Auth.py::TestAuth::test_GetLoggedInUser_without_domain - jwt.exceptions.DecodeError: Not enough segments
FAILED test/Services/test_Auth.py::TestAuth::test_token_required_invalid_token - RuntimeError: Working outside of request context.
FAILED test/Services/test_Auth.py::TestAuth::test_token_required_missing_token - RuntimeError: Working outside of request context.
FAILED test/Services/test_Auth.py::TestAuth::test_token_required_valid_token_authorized - RuntimeError: Working outside of request context.
FAILED test/Services/test_Auth.py::TestAuth::test_token_required_valid_token_empty_user_list - RuntimeError: Working outside of request context.
FAILED test/Services/test_Auth.py::TestAuth::test_token_required_valid_token_unauthorized - RuntimeError: Working outside of request context.
======================================= 7 failed, 70 passed, 10 warnings in 5.84s ======================================= 
PS C:\Sujith\Projects\SADRD\FinanceIT_SADRD\API>
