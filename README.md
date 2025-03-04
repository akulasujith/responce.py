import unittest
from unittest.mock import MagicMock
from flask import Flask, jsonify  # Import Flask and jsonify

# ... (your existing imports)

class TestApirespHandler(unittest.TestCase):
    def setUp(self):
        self.app = Flask(__name__)
        self.app_context = self.app.app_context()
        self.app_context.push()
        self.handler = apir.ApirespHandler()

    def tearDown(self):
        self.app_context.pop()

    def test_getResponse_empty_resp(self):
        mock_resp = MagicMock()
        mock_resp.__dict__ = {}
        self.handler.setResponse(mock_resp)
        response = self.handler.getResponse()
        # You might need to adjust the assertion based on the expected behavior
        self.assertEqual(response.status_code, 200)  # Check for a successful response

    # ... (rest of your test methods)
