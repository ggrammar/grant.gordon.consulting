# Mocking API Calls in Python

I like to write Python scripts to glue together various APIs. For example, I recently wrote a script to list VMs that were present on our hypervisor, but not our monitoring software. I can use the script output to install our monitoring agent on those VMs. 

It would be great if, during development, I didn't have to hammer the hypervisor API and monitoring API to test my script. Today, I'm exploring how to mock these API calls, to gain a number of benefits:
 - Faster development cycle (15s API call vs 15ms read from disk)
 - More reliable testing (data never changes, I won't be affected by API outages)
 - No danger of rate-limiting or locking myself out for looking like a DDos attack

## Simple Use Case

I'll start as simple as possible - a function and a test case. Normally tests live in separate files from their implementation, but I'll skip that for now for brevity.

```
import requests  # Simulate an API call
import unittest  # Python's built-in testing library

# This is the function we want to test. 
def call_third_party():
	return requests.get("https://time.gov")

# This is the unit test for that function - right now, we're actually issuing an
# HTTP requuest every time we run this test. 
class TestCallThirdParty(unittest.TestCase):
	def test_call_third_party(self):
		r = call_third_party()
		self.assertEqual(r.status_code, 200)
		self.assertTrue(r.text.contains("Federal government"))
```

I'll call this at the command line. 

```
% python3 -m unittest ./main.py
.
----------------------------------------------------------------------
Ran 1 test in 0.125s

OK
```

## Pickle the Response

Let's start with a simple improvement. How can we store the response of `requests.get`, and use that instead of an actual HTTP request during the test?

`requests.get` returns a Python object, which we can store with the `pickle` module. `pickle` allows us to serialize a Python object, which we can then write to disk. I'll write a separate script to do this. I can run the script once to retrieve the test data, then I can run the test as many times as I want. 

```
import pickle
import requests

r = requests.get("https://time.gov")
with open("./pickle", "wb") as f:
	f.write(pickle.dumps(r))
```

I'll modify the test script to use the pickled data, rather than hitting the API directly:

```
import pickle   # Serialize Python objects for storage
import requests
import unittest
from unittest import mock

# The function we're testing doesn't need to change, which is great! 
# Further, if we split up function definitions and tests, we could remove 
# `import requests`, which is cool. 
def call_third_party():
	return requests.get("https://time.gov") 

class TestCallThirdParty(unittest.TestCase):

	# The @mock.patch decorator allows us to mock one specific thing.
	# So, we need to have some insight into the function we're 
	# testing: we need to know that the function uses `requests.get`
	# internally. 
	# `my_cool_mock` is the object that's patched with the decorator.
	# It doesn't matter what it's called, as long as we reference it
	# by the same name in the function. It might be more helpful to
	# call it something called requests_get_mock or similar, but I 
	# didn't want it to look like the name was load-bearing. 
	# https://docs.python.org/3/library/unittest.mock.html#quick-guide
	@mock.patch('requests.get')
	def test_call_third_party(self, my_cool_mock):
		with open('./pickle', 'rb') as f:
			my_cool_mock.return_value = pickle.load(f)

		r = call_third_party()
		self.assertEqual(r.status_code, 200)
		self.assertTrue('Federal government' in r.text)
```

This works great, and at 0.005s, it runs about 20x faster than the 0.125s network call!

```
% python3 -m unittest ./main.py
.
----------------------------------------------------------------------
Ran 1 test in 0.005s

OK
```

## A Different Mock

Another option I found is to use the `requests-mock` library, which is purpose-built for mocking `requests`. Here's what that looks like:

```
import pickle
import requests
import requests_mock
import unittest

def call_third_party():
	return requests.get("https://time.gov")

class TestCallThirdParty(unittest.TestCase):

	# In this case, we set up a Mocker that intercepts the requests. 
	# There are a couple of ways to handle this - more reading:
	# https://pypi.org/project/requests-mock/
	@requests_mock.Mocker()
	def test_call_third_party(self, m):
		with open('./pickle', 'rb') as f:
			# Let requests_mock know the specific URL we're mocking.
			# This can be more flexible than @mock.patch, which (as
			# far as I know) can only override _all_ instances of
			# `request.get`. Also worth noting, we're only mocking
			# the .text property of the response (unlike @mock.patch,
			# where we're mocking the entire reponse object). It 
			# looks like you can mock only json, text, content, body,
			# and raw. 
			m.get("https://time.gov", text=(pickle.load(f)).text)

		r = call_third_party()
		self.assertEqual(r.status_code, 200)
		self.assertTrue('Federal government' in r.text)
```

This is a great start! We can store network responses on disk, and read them from disk during testing. We have a couple of valid approaches for mocking. There's also some neat things you can do with `setUp()` that allow you to mock all of the requests in a class, which I'll explore in a different post. 

