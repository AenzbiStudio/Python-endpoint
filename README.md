Sure! Here's an example of a Python endpoint using Flask that handles user sign-up and enforces strong password requirements. A strong password is defined as one that is at least 8 characters long, contains both uppercase and lowercase letters, includes at least one numerical digit, and has at least one special character.

```python
from flask import Flask, request, jsonify
import re

app = Flask(__name__)

def is_strong_password(password):
    # Password must be at least 8 characters long
    if len(password) < 8:
        return False
    # Password must contain at least one uppercase letter
    if not re.search(r'[A-Z]', password):
        return False
    # Password must contain at least one lowercase letter
    if not re.search(r'[a-z]', password):
        return False
    # Password must contain at least one digit
    if not re.search(r'\d', password):
        return False
    # Password must contain at least one special character
    if not re.search(r'[!@#$%^&*(),.?":{}|<>]', password):
        return False
    return True

@app.route('/signup', methods=['POST'])
def signup():
    data = request.get_json()
    
    # Extracting user information
    username = data.get('username')
    password = data.get('password')
    
    # Checking if username and password are provided
    if not username or not password:
        return jsonify({'message': 'Username and password are required!'}), 400
    
    # Checking if the password is strong
    if not is_strong_password(password):
        return jsonify({'message': 'Password is not strong enough!'}), 400
    
    # If strong password, proceed with the sign-up logic
    # For example, save the user to the database (this part is not implemented here)
    
    return jsonify({'message': 'User signed up successfully!'}), 201

if __name__ == '__main__':
    app.run(debug=True)
```

### Explanation:

1. **Password Strength Check**:
   - The `is_strong_password` function uses regular expressions to check the password's strength.
   - It ensures the password is at least 8 characters long, contains uppercase and lowercase letters, has digits, and includes special characters.

2. **Sign-Up Endpoint**:
   - The `/signup` endpoint accepts POST requests with JSON data containing `username` and `password`.
   - It checks if the username and password are provided.
   - It validates the password strength using the `is_strong_password` function.
   - If the password is strong, it proceeds with the sign-up process (e.g., saving the user to the database).

### Usage:
- To test this endpoint, you can use tools like Postman or curl to send a POST request to `http://127.0.0.1:5000/signup` with a JSON body like:
  ```json
  {
      "username": "user123",
      "password": "StrongP@ssw0rd!"
  }
  ```

This example assumes you have Flask installed. You can install Flask using `pip install Flask`.
