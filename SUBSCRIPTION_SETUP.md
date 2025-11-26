# Subscription Plan System Setup

## Overview
The subscription plan system allows users to have either a **Basic** or **Premium** subscription plan. All new users are automatically assigned the Basic plan by default.

## Features

### Basic Plan (Default)
- ✅ Access to all quizzes
- ✅ Bookmark questions
- ✅ View quiz results
- ✅ Track progress
- ✅ Resume incomplete quizzes

### Premium Plan
- ✅ Everything in Basic Plan
- ✅ AI Assistant access
- ✅ Priority support
- ✅ Advanced analytics

## Database Setup

After adding the subscription system, you need to create and run migrations:

```bash
python manage.py makemigrations quiz
python manage.py migrate
```

## Creating Profiles for Existing Users

If you have existing users, you can create UserProfile entries for them using the Django shell:

```bash
python manage.py shell
```

Then run:
```python
from django.contrib.auth.models import User
from quiz.models import UserProfile

# Create profiles for all existing users
for user in User.objects.all():
    UserProfile.objects.get_or_create(user=user)
    print(f"Created profile for {user.username}")
```

Or create a management command for this:
```python
# quiz/management/commands/create_user_profiles.py
from django.core.management.base import BaseCommand
from django.contrib.auth.models import User
from quiz.models import UserProfile

class Command(BaseCommand):
    help = 'Create UserProfile for all existing users'

    def handle(self, *args, **options):
        created_count = 0
        for user in User.objects.all():
            profile, created = UserProfile.objects.get_or_create(user=user)
            if created:
                created_count += 1
                self.stdout.write(self.style.SUCCESS(f'Created profile for {user.username}'))
        self.stdout.write(self.style.SUCCESS(f'Total profiles created: {created_count}'))
```

Run it with:
```bash
python manage.py create_user_profiles
```

## Usage

### For Users
1. Go to your Profile page
2. See your current subscription plan (Basic by default)
3. Click "Upgrade to Premium" to upgrade
4. Click "Downgrade to Basic" to downgrade

### For Administrators
1. Access Django Admin panel (`/admin/`)
2. Navigate to "Quiz" → "User Profiles"
3. View all users and their subscription plans
4. Edit subscription plans if needed

## Checking Subscription in Code

To check a user's subscription plan in your views or templates:

```python
# In views
from quiz.models import UserProfile

profile, created = UserProfile.objects.get_or_create(user=request.user)
if profile.is_premium:
    # User has premium access
    pass
elif profile.is_basic:
    # User has basic access
    pass

# Or using properties
if profile.subscription_plan == 'premium':
    # Premium user
    pass
```

In templates:
```django
{% if user_profile.subscription_plan == 'premium' %}
    <!-- Premium features -->
{% endif %}
```

## Restricting Features by Subscription

You can restrict features based on subscription. For example, to restrict AI Assistant to Premium users only:

```python
@login_required
def ai_assistant(request, session_id, question_id):
    # Check if user has premium subscription
    profile, created = UserProfile.objects.get_or_create(user=request.user)
    if not profile.is_premium:
        return JsonResponse({
            'error': 'Premium subscription required',
            'response': 'AI Assistant is only available for Premium users. Please upgrade your subscription.'
        }, status=403)
    
    # Continue with AI Assistant logic...
```

## Customization

### Adding New Plans
Edit `quiz/models.py` and add to `SUBSCRIPTION_CHOICES`:
```python
SUBSCRIPTION_CHOICES = [
    ('basic', 'Basic'),
    ('premium', 'Premium'),
    ('enterprise', 'Enterprise'),  # New plan
]
```

### Changing Default Plan
Edit `quiz/models.py`:
```python
subscription_plan = models.CharField(
    max_length=20,
    choices=SUBSCRIPTION_CHOICES,
    default='premium',  # Change default here
    ...
)
```

## Notes

- All new users automatically get a Basic plan when they register
- Users can freely upgrade/downgrade their plans from the Profile page
- Subscription plans are stored in the `UserProfile` model
- The system uses Django signals to automatically create profiles for new users

