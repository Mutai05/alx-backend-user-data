# alx-backend-user-data

Sure, let's go through each of the learning objectives step-by-step:

### 1. Examples of Personally Identifiable Information (PII)

**Personally Identifiable Information (PII)** refers to any data that could potentially identify a specific individual. Examples include:

- Full name
- Social Security Number (SSN)
- Driver's license number
- Passport number
- Email address
- Phone number
- Home address
- Credit card numbers
- Date of birth
- Biometric data (fingerprints, retina scans)
- IP address (when linked to other identifying information)

### 2. How to Implement a Log Filter that Will Obfuscate PII Fields

To protect sensitive information, it's important to obfuscate PII in logs. Here’s an example using Python’s logging module:

```python
import logging
import re

class PiiFilter(logging.Filter):
    PII_FIELDS = ['name', 'email', 'ssn', 'phone']

    def filter(self, record):
        record.msg = self.obfuscate_pii(record.msg)
        return True

    def obfuscate_pii(self, msg):
        for field in self.PII_FIELDS:
            msg = re.sub(f'({field}=)[^& ]+', r'\1[REDACTED]', msg)
        return msg

logger = logging.getLogger('my_logger')
logger.setLevel(logging.DEBUG)

handler = logging.StreamHandler()
handler.addFilter(PiiFilter())

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)

logger.addHandler(handler)

logger.info("User info: name=John Doe, email=john.doe@example.com, ssn=123-45-6789, phone=555-1234")
```

### 3. How to Encrypt a Password and Check the Validity of an Input Password

To encrypt passwords and verify them, you can use the `bcrypt` package in Python:

1. **Install bcrypt**:
   ```bash
   pip install bcrypt
   ```

2. **Encrypting a password**:
   ```python
   import bcrypt

   def hash_password(password):
       salt = bcrypt.gensalt()
       hashed = bcrypt.hashpw(password.encode(), salt)
       return hashed

   password = "my_secure_password"
   hashed_password = hash_password(password)
   print(hashed_password)
   ```

3. **Checking the validity of an input password**:
   ```python
   def check_password(input_password, stored_hashed_password):
       return bcrypt.checkpw(input_password.encode(), stored_hashed_password)

   input_password = "my_secure_password"
   print(check_password(input_password, hashed_password))
   ```

### 4. How to Authenticate to a Database Using Environment Variables

Using environment variables to store sensitive information like database credentials is a good practice. Here’s an example using Python and MongoDB:

1. **Set environment variables**:
   ```bash
   export DB_USERNAME='your_db_username'
   export DB_PASSWORD='your_db_password'
   export DB_HOST='your_db_host'
   export DB_NAME='your_db_name'
   ```

2. **Authenticate to the database**:
   ```python
   import os
   from pymongo import MongoClient

   db_username = os.getenv('DB_USERNAME')
   db_password = os.getenv('DB_PASSWORD')
   db_host = os.getenv('DB_HOST')
   db_name = os.getenv('DB_NAME')

   uri = f"mongodb+srv://{db_username}:{db_password}@{db_host}/{db_name}?retryWrites=true&w=majority"
   client = MongoClient(uri)
   db = client[db_name]

   print("Connected to the database:", db)
   ```

This approach ensures that sensitive information is not hard-coded into your scripts, enhancing security.
