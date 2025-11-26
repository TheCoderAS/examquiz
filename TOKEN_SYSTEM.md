# Token System Documentation

## Overview
The token system allows users to use premium features like AI Assistant. Basic users receive 100 tokens when they create an account, and Premium users receive an additional 1000 tokens when they upgrade.

## Token Allocation

### Basic Plan
- **Initial Tokens**: 100 tokens upon account creation
- **AI Assistant Cost**: 1 token per request

### Premium Plan  
- **Initial Tokens**: 100 tokens (inherited from Basic)
- **Upgrade Bonus**: +1000 tokens when upgrading to Premium
- **Total**: 1100 tokens (after upgrade)
- **AI Assistant Cost**: 1 token per request

## Features

### UserProfile Model
- Added `tokens` field (PositiveIntegerField, default=100)
- Helper methods:
  - `add_tokens(amount)` - Add tokens to account
  - `deduct_tokens(amount)` - Deduct tokens (returns True/False)
  - `has_tokens(amount=1)` - Check if user has sufficient tokens

### Token Management
1. **New User Registration**: Automatically receives 100 tokens via Django signals
2. **Upgrade to Premium**: Adds 1000 tokens automatically
3. **AI Assistant Usage**: Deducts 1 token per request (after successful response)

## Usage in Code

### Check Token Balance
```python
from quiz.models import UserProfile

profile, created = UserProfile.objects.get_or_create(user=request.user)
if profile.has_tokens(1):
    # User has enough tokens
    pass
```

### Deduct Tokens
```python
if profile.deduct_tokens(1):
    # Successfully deducted 1 token
    print(f"Tokens remaining: {profile.tokens}")
else:
    # Insufficient tokens
    print("Not enough tokens")
```

### Add Tokens
```python
profile.add_tokens(100)  # Adds 100 tokens
```

## Token Display

### Profile Page
- Token balance is displayed in the user profile card
- Shows current token count with icon
- Displays cost information (1 token per AI Assistant request)

### Admin Panel
- Token balance visible in UserProfile list
- Admin actions to add tokens:
  - Add 100 tokens
  - Add 500 tokens
  - Add 1000 tokens
- Admin actions to reset tokens:
  - Reset to 100 (Basic)
  - Reset to 1100 (Premium)

## AI Assistant Integration

### Token Check
Before processing an AI Assistant request:
```python
TOKENS_PER_AI_REQUEST = 1
if not profile.has_tokens(TOKENS_PER_AI_REQUEST):
    return JsonResponse({
        'error': 'Insufficient tokens',
        'response': f'You have {profile.tokens} token(s) remaining...'
    }, status=403)
```

### Token Deduction
After successful AI response:
```python
profile.deduct_tokens(TOKENS_PER_AI_REQUEST)
return JsonResponse({
    'success': True,
    'response': ai_response,
    'tokens_remaining': profile.tokens
})
```

## Migration Instructions

After implementing the token system:

1. **Create and run migrations**:
```bash
python manage.py makemigrations quiz
python manage.py migrate
```

2. **Set tokens for existing users**:
```python
# In Django shell or management command
from django.contrib.auth.models import User
from quiz.models import UserProfile

for user in User.objects.all():
    profile, created = UserProfile.objects.get_or_create(user=user)
    if created:
        profile.tokens = 100
        profile.save()
    elif profile.tokens == 0:
        # Set tokens for existing profiles without tokens
        profile.tokens = 100
        profile.save()
```

## Customization

### Change Initial Token Amount
Edit `quiz/models.py`:
```python
tokens = models.PositiveIntegerField(
    default=100,  # Change this value
    ...
)
```

### Change Premium Upgrade Bonus
Edit `quiz/views.py` in `upgrade_subscription()`:
```python
profile.add_tokens(1000)  # Change this value
```

### Change AI Assistant Cost
Edit `quiz/views.py` in `ai_assistant()`:
```python
TOKENS_PER_AI_REQUEST = 1  # Change this value
```

## Future Enhancements

Potential improvements:
- Token purchase/payment integration
- Token expiration dates
- Token usage analytics
- Daily token refills for premium users
- Token rewards for completing quizzes
- Token transaction history

