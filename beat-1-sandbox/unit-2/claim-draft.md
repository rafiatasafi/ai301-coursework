Hey! I'd like to take this one on — verify_password in core/security.py lets passlib's UnknownHashError bubble up when it hits a hash it can't recognize, instead of failing closed and returning False.

I will reproduce it locally first: throw a malformed/unrecognized hash at verify_password and confirm the exception surfaces instead of returning a clean False. There's an @pytest.mark.xfail test in tests/unit/test_security.py (referencing manifest H-05) that looks like the right anchor point; I'll start there and report back with what I find.
