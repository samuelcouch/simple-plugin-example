# Plugin Example
This example plugin is best used when following along with the [Build Your First Plugin](https://jackhenry.dev/open-api-docs/plugins/quickstarts/BuildYourFirstPlugin/) quickstart.

## Architecture
The authentication flow has been refactored into a modular Express structure with two main entry points:

### Main Routes
- **`/static`** - Static showcase page (no authentication required)
  - Demonstrates a simple plugin page that loads immediately
  - Includes a link to try the authenticated experience
  - Perfect for testing basic plugin functionality

- **`/auth`** - Authenticated plugin experience
  - Initiates OIDC/PKCE authentication flow
  - Redirects through OAuth for user login
  - Displays personalized account information after authentication

### Internal Routes
- **`/auth/callback`** - OAuth callback handler (internal use)
  - Handles the redirect from Banno's authorization server
  - Exchanges authorization code for access tokens
  - Renders the dynamic content with user data

### File Structure
```
├── controllers/authController.js    # Authentication logic
├── routes/auth.js                   # Auth route definitions  
├── utils/tokenService.js            # Token exchange utilities
├── utils/pkce.js                    # PKCE helper functions
└── server.js                        # Express app bootstrap
```

## How to Run
1. Install dependencies `npm install`
1. Rename `config-EXAMPLE.js` to `config.js`
1. Add `client_id`, `client_secret`, and `environment` to the config file
1. Ensure the port set in `config.js` is available on your system (8080 by default)
1. **Configure External Application in Banno People:**
   - **Primary redirect URI**: `http://localhost:8080/auth` (this is your main plugin URL)
   - **Additional redirect URIs**: Add `http://localhost:8080/auth/callback` for OAuth callback
   - **Important**: Use `/auth` as the primary redirect URI, not `/auth/callback`
   - The `/static` route works without any OAuth configuration
1. Add the plugin to the user dashboard
1. Run the app with `npm run start`

## Testing the Plugin
Once running, you can test both plugin experiences:

- **Static Plugin**: Visit `http://localhost:8080/static`
  - Loads immediately without authentication
  - Shows basic plugin functionality

- **Authenticated Plugin**: Visit `http://localhost:8080/auth`
  - Redirects through OAuth flow for user login
  - Displays personalized account information
  - Requires proper External Application configuration

## External Application Configuration

**Critical Setup Requirements:**

1. **Primary Redirect URI**: `http://localhost:8080/auth`
   - This is your main plugin URL that Banno will load
   - **Do NOT** use `/auth/callback` as the primary redirect URI

2. **Additional Redirect URIs**: 
   - `http://localhost:8080/auth/callback` (for OAuth callback)
   - `http://localhost:8080/static` (optional, for static plugin)

**Common Issue**: If you get "Auth error: Missing code or state", check that `http://localhost:8080/auth` is your primary redirect URI, not `http://localhost:8080/auth/callback`.
