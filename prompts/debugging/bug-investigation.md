# Bug Investigation Prompt

Use this template to get help investigating and fixing bugs.

## Prompt Template

```
I'm encountering a bug and need help investigating and fixing it.

## Bug Description
**Summary**: [Brief description of the issue]
**Expected Behavior**: [What should happen]
**Actual Behavior**: [What actually happens]
**Frequency**: [Always, Sometimes, Rarely]

## Environment
- **OS**: [Windows, macOS, Linux]
- **Language/Framework**: [Version numbers]
- **Dependencies**: [Relevant package versions]
- **Environment**: [Development, Staging, Production]

## Steps to Reproduce
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Code Involved
```[language]
[Paste relevant code]
```

## Error Messages/Logs
```
[Paste error messages, stack traces, or relevant logs]
```

## What I've Tried
- [Attempt 1 and result]
- [Attempt 2 and result]

## Additional Context
[Any other relevant information]

## Investigation Requests
Please help me:
1. Identify the root cause of the bug
2. Explain why it's happening
3. Suggest multiple potential fixes
4. Recommend the best approach
5. Provide code to implement the fix
6. Suggest how to prevent similar bugs
```

## Example Usage

```
I'm encountering a bug and need help investigating and fixing it.

## Bug Description
**Summary**: User logout fails intermittently, leaving sessions active
**Expected Behavior**: When user clicks logout, session is destroyed and user is redirected to login page
**Actual Behavior**: Sometimes logout succeeds, but ~30% of the time the session remains active and user can still access protected pages
**Frequency**: Intermittent (happens about 30% of logout attempts)

## Environment
- **OS**: Ubuntu 22.04 (production), macOS (development)
- **Language/Framework**: Node.js 18.x, Express 4.18.2
- **Dependencies**: 
  - express-session: 1.17.3
  - connect-redis: 7.1.0
  - redis: 4.6.5
- **Environment**: Happens in both staging and production

## Steps to Reproduce
1. Login with valid credentials
2. Navigate to dashboard (protected page)
3. Click logout button
4. Observe sometimes redirected to login, sometimes can still access dashboard
5. Cannot reliably reproduce - seems random

## Code Involved
```javascript
// Logout route
app.post('/auth/logout', async (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      console.error('Session destruction error:', err);
      return res.status(500).json({ error: 'Logout failed' });
    }
    res.clearCookie('connect.sid');
    res.json({ message: 'Logged out successfully' });
  });
});

// Session configuration
app.use(session({
  store: new RedisStore({ client: redisClient }),
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: false,
    httpOnly: true,
    maxAge: 1000 * 60 * 60 * 24 // 24 hours
  }
}));

// Auth middleware
const requireAuth = (req, res, next) => {
  if (req.session && req.session.userId) {
    next();
  } else {
    res.status(401).json({ error: 'Unauthorized' });
  }
};

// Protected route
app.get('/dashboard', requireAuth, (req, res) => {
  res.render('dashboard');
});
```

## Error Messages/Logs
```
# No errors in application logs during failed logout
# Redis logs show:
[INFO] DEL session:abc123
[INFO] Connection closed by client

# Browser console shows successful logout response:
{message: "Logged out successfully"}

# But subsequent request to /dashboard still works:
GET /dashboard HTTP/1.1
Cookie: connect.sid=s%3Aabc123.xyz789
Response: 200 OK (dashboard content)
```

## What I've Tried
- Added explicit res.clearCookie() call - didn't fix it
- Increased Redis connection pool size - didn't help
- Tried session.destroy() with and without callback - same issue
- Checked Redis - session keys are being deleted successfully
- Added logging - session.destroy() callback fires without errors

## Additional Context
- Using load balancer (Nginx) with 3 Node.js instances
- Redis is shared across all instances (single Redis server)
- Problem started after scaling from 1 to 3 instances
- Did not occur when running single instance
- Browser is sending requests to different backend instances

## Investigation Requests
Please help me:
1. Identify the root cause of the bug
2. Explain why it's happening (especially the load balancer/multiple instance angle)
3. Suggest multiple potential fixes
4. Recommend the best approach for production
5. Provide code to implement the fix
6. Suggest how to prevent similar bugs (testing strategies, monitoring)
```

## Tips

- **Specificity**: More detail helps with diagnosis
- **Minimal Example**: Reduce code to minimal reproducible case
- **Logs**: Include full error messages and stack traces
- **Environment**: Mention anything unusual about your setup
- **Investigation**: Show what you've already tried
