### Bug Fixes

* Fixed a bug in `src/flask/ctx.py` where `appcontext_popped` signal was sent even if no context was popped. This could lead to incorrect behavior in certain edge cases, especially when nested application contexts were involved.

