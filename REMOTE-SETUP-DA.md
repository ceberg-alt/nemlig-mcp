# Sikker Claude-forbindelse fra iPhone

Denne version beskytter `/mcp` med `Authorization: Bearer <MCP_API_KEY>`.
Serveren nægter at starte i produktion uden en adgangsnøgle på mindst 32 tegn.
Brug **ikke** nemlig-adgangskoden som MCP_API_KEY.

1. Brug Render til at oprette en web service fra din *private* GitHub-fork. Vælg Docker runtime og en betalt plan hvis stabil drift er vigtig. Alternativt Blueprint via `render.yaml`. Tjek aktuel pris før valg.
2. Angiv hemmelige miljøvariabler i Renders dashboard (aldrig i GitHub): `MCP_API_KEY` = en lang tilfældig nøgle, `NEMLIG_USERNAME` og `NEMLIG_PASSWORD` = dit nemlig-login. Sæt `NODE_ENV=production` og `NEMLIG_ALLOW_HEADER_CREDENTIALS=false`.
3. Vent på successful deployment. Health-check: `https://DIN-SERVER.onrender.com/health` (ingen login-data må vises). `/mcp` skal give HTTP 401 uden Bearer-token.
4. I Claude på web: Customize > Connectors > Add custom connector. URL: `https://DIN-SERVER.onrender.com/mcp`. Vælg 'No sign in' og tilføj under Request headers `Authorization` med værdien `Bearer DIN-HEMMELIGE-MCP_API_KEY` (med mellemrum efter Bearer). Gem.
5. Slå forbindelsen til i Claude, og bed først om at **læse** kurven. Test derefter tilføjelse af én vare, og tjek kurven i nemlig-appen.

**Sikkerhed:** Fortæl aldrig Claude/chatten dine kodeord eller API-nøgler. Render-miljøvariabler skal blive hemmelige. MCP-nøglen kan tilbagekaldes ved at rotere `MCP_API_KEY`. Denne adgangsnøgle beskytter mod uautoriserede kald, men løser ikke alle risici ved hosting af tredjepartskode. Hvis du mistænker misbrug, deaktiver tjenesten og skift begge adgangskoder. Ingen betaling eller ordreafgivelse er implementeret.

**Begrænsning:** Gratis Render-tjenester kan sove og have midlertidig lagerplads. Forbindelsen er ikke testet mod din rigtige nemlig-konto. Nemlig tilbyder ikke et offentligt API, og deres interne API kan ændre sig.
