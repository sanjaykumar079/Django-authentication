# Django Authentication Project

A complete Django web application with user authentication features including registration, login, logout, and password reset functionality with email notifications.

## Features

- **User Registration** - Create new user accounts with validation
- **User Login/Logout** - Secure authentication system
- **Password Reset** - Email-based password reset with time-limited tokens
- **Form Validation** - Client and server-side validation
- **Responsive Design** - Modern CSS styling with gradient backgrounds
- **Email Integration** - Gmail SMTP configuration for password reset emails

## Project Structure

```
full_authentication/
├── project/
│   ├── core/                    # Main application
│   │   ├── migrations/
│   │   ├── models.py           # PasswordReset model
│   │   ├── views.py            # Authentication views
│   │   ├── urls.py             # URL routing
│   │   ├── admin.py            # Admin configuration
│   │   └── apps.py
│   ├── project/                # Project settings
│   │   ├── settings.py         # Django settings
│   │   ├── urls.py             # Main URL configuration
│   │   └── wsgi.py
│   ├── templates/              # HTML templates
│   │   ├── index.html          # Home page
│   │   ├── login.html          # Login form
│   │   ├── register.html       # Registration form
│   │   ├── forgot_password.html # Password reset request
│   │   ├── password_reset_sent.html # Reset confirmation
│   │   └── reset_password.html # New password form
│   ├── static/
│   │   └── style.css           # Custom styling
│   └── manage.py
```

## Installation & Setup

### Prerequisites
- Python 3.8+
- Django 5.0.7
- Gmail account for email functionality

### 1. Clone the Repository
```bash
git clone <repository-url>
cd full_authentication/project
```

### 2. Create Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install django
```

### 4. Database Setup
```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Create Superuser (Optional)
```bash
python manage.py createsuperuser
```

### 6. Email Configuration
Update the email settings in `project/settings.py`:

```python
EMAIL_HOST = "smtp.gmail.com"
EMAIL_PORT = 465
EMAIL_USE_SSL = True
EMAIL_HOST_USER = "your-email@gmail.com"
EMAIL_HOST_PASSWORD = "your-app-password"  # Use App Password, not regular password
```

**Important**: For Gmail, you'll need to:
1. Enable 2-factor authentication
2. Generate an App Password
3. Use the App Password in `EMAIL_HOST_PASSWORD`

### 7. Run the Server
```bash
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` to access the application.

## Usage

### User Registration
- Navigate to `/register/`
- Fill out the registration form with:
  - First Name
  - Last Name
  - Username (must be unique)
  - Email (must be unique)
  - Password (minimum 5 characters)

### User Login
- Navigate to `/login/`
- Enter username and password
- Successful login redirects to home page

### Password Reset
1. Navigate to `/forgot-password/`
2. Enter your email address
3. Check your email for reset link
4. Click the link (valid for 10 minutes)
5. Enter new password
6. Login with new credentials

## Security Features

- **CSRF Protection** - All forms include CSRF tokens
- **Password Validation** - Minimum length requirements
- **Email Verification** - Password reset requires valid email
- **Time-Limited Tokens** - Reset links expire after 10 minutes
- **Input Validation** - Server-side validation for all forms
- **Unique Constraints** - Prevents duplicate usernames/emails

## Models

### PasswordReset
```python
class PasswordReset(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    reset_id = models.UUIDField(default=uuid.uuid4, unique=True, editable=False)
    created_when = models.DateTimeField(auto_now_add=True)
```

## URL Routes

- `/` - Home page (login required)
- `/register/` - User registration
- `/login/` - User login
- `/logout/` - User logout
- `/forgot-password/` - Password reset request
- `/password-reset-sent/<reset_id>/` - Reset confirmation
- `/reset-password/<reset_id>/` - New password form

## Customization

### Styling
- Modify `static/style.css` for custom styling
- Templates use modern gradient backgrounds and responsive design
- Poppins font family from Google Fonts

### Email Templates
- Email body is defined in `views.py` in the `Forgot_password` view
- Customize the email message format as needed

### Validation Rules
- Password minimum length: 5 characters (configurable in views)
- Username and email uniqueness enforced
- Additional validation can be added in views

## Troubleshooting

### Common Issues

1. **Email not sending**
   - Check Gmail App Password configuration
   - Verify 2FA is enabled on Gmail account
   - Check spam folder

2. **Static files not loading**
   - Run `python manage.py collectstatic`
   - Check `STATIC_URL` and `STATICFILES_DIRS` in settings

3. **Reset link expired**
   - Links are valid for 10 minutes only
   - Request a new reset if expired

4. **Database errors**
   - Run migrations: `python manage.py migrate`
   - Check database permissions

## Production Deployment

Before deploying to production:

1. Set `DEBUG = False` in settings.py
2. Update `ALLOWED_HOSTS` with your domain
3. Use environment variables for sensitive data
4. Configure proper email backend
5. Set up proper static file serving
6. Use a production database (PostgreSQL recommended)

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is open source and available under the [MIT License](LICENSE).

## Support

For support, please open an issue in the repository or contact the development team.
