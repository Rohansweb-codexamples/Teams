# Rohans Web Teams

Auth0-secured team workspace with team invite codes, team chat, saved whiteboards and a simple design editor. Node.js/Express serves the frontend and API; MongoDB Atlas persists content.

## 1. Set up Auth0
1. Sign in at https://manage.auth0.com/ and open Applications → Applications → Create Application.
2. Name it **Rohans Web Teams** and choose **Single Page Web Applications**.
3. Under Settings, set Allowed Callback URLs, Allowed Logout URLs and Allowed Web Origins to `http://localhost:3000` for local testing. Add the exact production HTTPS origin to all three fields after deployment, then save.
4. Copy the Auth0 **Domain** and **Client ID**. The Client ID is public; never put a Client Secret in browser code.
5. Open Applications → APIs → Create API. Name it **Rohans Web Teams API**, set Identifier to `https://teams-api.rohansweb.co.uk`, choose RS256, and create it. Use this exact value for `AUTH0_AUDIENCE`. The identifier is an audience value and doesn't need to host a website. Guide: https://auth0.com/docs/get-started/apis.

## 2. Set up MongoDB Atlas
1. Create a cluster at https://www.mongodb.com/atlas and create a database user with a strong password.
2. In Network Access, allow your hosting provider to connect. Use a deliberate production network policy.
3. Copy the Drivers connection URI and replace username/password placeholders. Keep the URI private. The app creates its database and indexes on startup.

## 3. Environment variables
Copy `.env.example` to `.env` and set:
- `AUTH0_DOMAIN`: tenant domain without https://
- `AUTH0_CLIENT_ID`: SPA Client ID
- `AUTH0_AUDIENCE`: API Identifier from Auth0
- `MONGODB_URI`: private Atlas connection string
- `MONGODB_DB=rohansweb_teams`
- `PORT=3000`

Never commit `.env` or expose `MONGODB_URI`.

## 4. Run locally
Requires Node.js 20 or newer.
```bash
npm install
cp .env.example .env
# Edit .env with your Auth0 and MongoDB values
npm run dev
```
Open http://localhost:3000.

## 5. Deploy to Render
1. Create a Web Service connected to https://github.com/Rohansweb-codexamples/Teams.
2. Build command: `npm install`; Start command: `npm start`.
3. Add all environment variables in Render's Environment settings. Do not upload your real .env.
4. Deploy, then add the exact Render service URL to Auth0's callback, logout and web-origin allowlists.
5. If attaching a custom domain, configure DNS with your host and add that exact HTTPS origin to Auth0.

GitHub Pages alone cannot run this Node API; use a Node-capable host.

## Included
- Auth0 hosted login and signup
- Create teams and share invite codes; join with a code
- Team-scoped chat stored in MongoDB
- Whiteboard pen and sticky notes, save/load/delete
- Design canvas with text, shapes, draggable objects, background colours, PNG export and save/load/delete
- Personal or team-associated saved items

## Important limitations
Chat refreshes every seven seconds rather than using WebSockets. Live simultaneous whiteboard collaboration, attachments, calls, presence, expiring invites, rate limiting and advanced team roles are not included yet. Share invite codes only with intended people. The API validates Auth0 access tokens and checks team membership before returning team content.
