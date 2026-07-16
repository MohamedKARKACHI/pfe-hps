====== dashboard/report/diagrams_src\diagram_1.tex ======
\begin{tikzpicture}[node distance=1.2cm and 1.8cm]
  % ── Frontend Layer ──
  \node[ndBlue, minimum width=4cm, minimum height=1.2cm] (react) {\textbf{React + Vite}\\\footnotesize Port 5175};
  \node[ndBlue, minimum width=2.8cm, right=1cm of react] (monaco) {Monaco Editor};
  \node[ndBlue, minimum width=2.8cm, right=0.5cm of monaco] (forms) {Formulaires\\Dynamiques};
  \node[ndBlue, minimum width=2.8cm, right=0.5cm of forms] (terminal) {Terminal\\SSE};

  \begin{scope}[on background layer]
    \node[bxBlue, fit=(react)(monaco)(forms)(terminal), label={[lB]above:Frontend (Dashboard React)}] (fbox) {};
  \end{scope}

  % ── Backend Layer ──
  \node[ndGreen, minimum width=3cm, below=2cm of react] (gateway) {\textbf{API Gateway}\\\footnotesize Port 8000};
  \node[ndGreen, minimum width=2.5cm, right=0.8cm of gateway] (ref) {Referential\\\footnotesize Port 8001};
  \node[ndGreen, minimum width=2.5cm, right=0.5cm of ref] (gen) {Generator\\\footnotesize Port 8002};
  \node[ndGreen, minimum width=2.5cm, right=0.5cm of gen] (runner) {Runner\\\footnotesize Port 8003};
  \node[ndGreen, minimum width=2.5cm, right=0.5cm of runner] (connect) {Connect\\\footnotesize Port 8004};

  \begin{scope}[on background layer]
    \node[bxGreen, fit=(gateway)(ref)(gen)(runner)(connect), label={[lG]above:Backend (Microservices Node.js)}] (bbox) {};
  \end{scope}

  % ── Cypress Layer ──
  \node[ndPurple, minimum width=3cm, below=2cm of ref] (specs) {Specs\\(.cy.js)};
  \node[ndPurple, minimum width=3cm, right=0.8cm of specs] (fixtures) {Fixtures\\(.json)};
  \node[ndPurple, minimum width=3cm, right=0.8cm of fixtures] (po) {Page Objects\\Commands};

  \begin{scope}[on background layer]
    \node[bxPurple, fit=(specs)(fixtures)(po), label={[lP]above:Projet Cypress E2E}] (cbox) {};
  \end{scope}

  % ── External ──
  \node[ndOrange, minimum width=3.5cm, below=2cm of fixtures] (pwcapi) {\textbf{PowerCard}\\\textbf{Connect API}};
  \node[ndOrange, minimum width=3.5cm, right=1.5cm of pwcapi] (pwcapp) {\textbf{PowerCard V4}\\\textbf{Application}};

  \begin{scope}[on background layer]
    \node[bxOrange, fit=(pwcapi)(pwcapp), label={[lO]above:Services Externes}] (ebox) {};
  \end{scope}

  % ── Arrows ──
  \draw[arw] (react) -- node[lbl]{Vite Proxy /api/*} (gateway);
  \draw[arw] (gateway) -- (ref);
  \draw[arw] (gateway) -- (gen);
  \draw[arw] (gateway) -- (runner);
  \draw[arw] (gateway) -- (connect);
  \draw[arw] (gen) -- node[lbl, left]{write} (specs);
  \draw[arw] (gen) -- node[lbl]{write} (fixtures);
  \draw[arw] (ref) -- node[lbl, left]{read} (specs);
  \draw[arw] (runner) -- node[lbl, right]{exec} (po);
  \draw[arw] (connect) -- node[lbl]{JWT Auth} (pwcapi);
  \draw[arw] (runner.south) -- ++(0,-0.5) -| node[lbl, near start]{tests E2E} (pwcapp);
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_2.tex ======
\begin{tikzpicture}[node distance=0.7cm]
  % ── Actors ──
  \node[actG] (tester) {QA};
  \node[below=0.1cm of tester, font=\tiny\bfseries, text=accent] {Testeur QA};

  \node[actP, right=12cm of tester] (admin) {ADM};
  \node[below=0.1cm of admin, font=\tiny\bfseries, text=purpleC] {Admin};

  % ── Use Cases (Testeur) ──
  \node[ucG, right=2cm of tester] (uc1) {UC1: Sélectionner un écran};
  \node[ucG, below=0.5cm of uc1] (uc2) {UC2: Remplir le formulaire};
  \node[ucG, below=0.5cm of uc2] (uc3) {UC3: Auto-Fill API};
  \node[ucG, below=0.5cm of uc3] (uc4) {UC4: Charger cas prédéfini};
  \node[ucG, below=0.5cm of uc4] (uc5) {UC5: Générer JSON};
  \node[ucG, below=0.5cm of uc5] (uc6) {UC6: Générer Spec Cypress};
  \node[ucG, below=0.5cm of uc6] (uc7) {UC7: Copier / Télécharger};
  \node[ucG, below=0.5cm of uc7] (uc8) {UC8: Exporter vers projet};
  \node[ucG, below=0.5cm of uc8] (uc9) {UC9: Exécuter test};
  \node[ucG, below=0.5cm of uc9] (uc10) {UC10: Voir rapport visuel};
  \node[ucG, below=0.5cm of uc10] (uc11) {UC11: Ouvrir page PowerCard};

  % ── Use Cases (Admin) ──
  \node[ucB, left=2cm of admin] (uc12) {UC12: Scanner specs};
  \node[ucB, below=0.5cm of uc12] (uc13) {UC13: Inférer templates};
  \node[ucB, below=0.5cm of uc13] (uc14) {UC14: Générer catalogue};

  % ── System boundary ──
  \begin{scope}[on background layer]
    \node[rectangle, rounded corners=8pt, draw=grayC, fill=graylight, fit=(uc1)(uc2)(uc3)(uc4)(uc5)(uc6)(uc7)(uc8)(uc9)(uc10)(uc11)(uc12)(uc13)(uc14), inner sep=12pt, label={[font=\small\bfseries, text=primary]above:QA Cypress Dashboard}] {};
  \end{scope}

  % ── Actor connections ──
  \foreach \uc in {uc1,uc2,uc3,uc4,uc5,uc6,uc7,uc8,uc9,uc10,uc11}
    \draw[arw, color=accent] (tester) -- (\uc);
  \foreach \uc in {uc12,uc13,uc14}
    \draw[arw, color=purpleC] (admin) -- (\uc);

  % ── Include relationships ──
  \draw[darw, color=blueC] (uc5) -- node[lbl]{\scriptsize\guillemotleft include\guillemotright} (uc2);
  \draw[darw, color=blueC] (uc8) -- node[lbl]{\scriptsize\guillemotleft include\guillemotright} (uc5);
  \draw[darw, color=blueC] (uc9) -- node[lbl]{\scriptsize\guillemotleft include\guillemotright} (uc8);
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_3.tex ======
\begin{tikzpicture}
  % ── Lifelines ──
  \node[ndBlue, minimum width=2.2cm] (user) {\textbf{Testeur QA}};
  \node[ndGreen, minimum width=2.2cm, right=2.5cm of user] (dash) {\textbf{Dashboard}};
  \node[ndPurple, minimum width=2.2cm, right=2.5cm of dash] (api) {\textbf{API Server}};
  \node[ndOrange, minimum width=2.2cm, right=2.5cm of api] (connect) {\textbf{Connect API}};
  \node[ndGreenC, minimum width=2.2cm, right=2.5cm of connect] (fs) {\textbf{File System}};

  % ── Lifeline bars ──
  \foreach \n in {user,dash,api,connect,fs}
    \draw[dashed, gray!50] (\n.south) -- ++(0,-14);

  % ── Phase 1: Sélection ──
  \node[snote, below=0.8cm of dash] (p1) {\textbf{Phase 1: Sélection}};
  \draw[arw, blueC] ([yshift=-1.5cm]user.south) -- node[lbl, above]{1. Sélectionner écran} ([yshift=-1.5cm]dash.south);
  \draw[arw, accent] ([yshift=-2.2cm]dash.south) -- node[lbl, above]{2. Charger template} ([yshift=-2.2cm]api.south);
  \draw[arw, accent, dashed] ([yshift=-2.8cm]api.south) -- node[lbl, above]{3. template + cas prédéfinis} ([yshift=-2.8cm]dash.south);

  % ── Phase 2: Auto-Fill ──
  \node[snote, below=4cm of dash] (p2) {\textbf{Phase 2: Auto-Fill}};
  \draw[arw, blueC] ([yshift=-4.5cm]user.south) -- node[lbl, above]{4. Clic Auto-Fill} ([yshift=-4.5cm]dash.south);
  \draw[arw, accent] ([yshift=-5.2cm]dash.south) -- node[lbl, above]{5. GET /api/connect/random-test-card} ([yshift=-5.2cm]api.south);
  \draw[arw, orangeC] ([yshift=-5.8cm]api.south) -- node[lbl, above]{6. Auth JWT + search\_card} ([yshift=-5.8cm]connect.south);
  \draw[arw, orangeC, dashed] ([yshift=-6.4cm]connect.south) -- node[lbl, above]{7. cardNumber, expDate} ([yshift=-6.4cm]api.south);
  \draw[arw, accent, dashed] ([yshift=-7cm]api.south) -- node[lbl, above]{8. données carte} ([yshift=-7cm]dash.south);

  % ── Phase 3: Génération ──
  \node[snote, below=8cm of dash] (p3) {\textbf{Phase 3: Génération}};
  \draw[arw, blueC] ([yshift=-8.5cm]user.south) -- node[lbl, above]{9. Clic Générer} ([yshift=-8.5cm]dash.south);
  \draw[arw, accent, dotted] ([yshift=-9.2cm]dash.south) -- ++(0,-0.5) node[lbl, right]{\footnotesize buildPayload() + patchSpec()};

  % ── Phase 4: Export ──
  \node[snote, below=10.5cm of dash] (p4) {\textbf{Phase 4: Export}};
  \draw[arw, blueC] ([yshift=-11cm]user.south) -- node[lbl, above]{10. Clic Exporter} ([yshift=-11cm]dash.south);
  \draw[arw, accent] ([yshift=-11.7cm]dash.south) -- node[lbl, above]{11. POST /api/export-to-project} ([yshift=-11.7cm]api.south);
  \draw[arw, greenC] ([yshift=-12.3cm]api.south) -- node[lbl, above]{12. writeFileSync()} ([yshift=-12.3cm]fs.south);
  \draw[arw, greenC, dashed] ([yshift=-12.9cm]fs.south) -- node[lbl, above]{13. OK} ([yshift=-12.9cm]api.south);
  \draw[arw, accent, dashed] ([yshift=-13.5cm]api.south) -- node[lbl, above]{14. success} ([yshift=-13.5cm]dash.south);
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_4.tex ======
\begin{tikzpicture}
  % ── Lifelines ──
  \node[ndBlue, minimum width=2.5cm] (user2) {\textbf{Testeur QA}};
  \node[ndGreen, minimum width=2.5cm, right=3cm of user2] (dash2) {\textbf{Dashboard}};
  \node[ndPurple, minimum width=2.5cm, right=3cm of dash2] (runner2) {\textbf{Runner Service}};
  \node[ndOrange, minimum width=2.5cm, right=3cm of runner2] (cypress2) {\textbf{Cypress Process}};

  \foreach \n in {user2,dash2,runner2,cypress2}
    \draw[dashed, gray!50] (\n.south) -- ++(0,-11);

  \draw[arw, blueC] ([yshift=-1cm]user2.south) -- node[lbl, above]{1. Clic Exécuter} ([yshift=-1cm]dash2.south);
  \draw[arw, accent] ([yshift=-1.8cm]dash2.south) -- node[lbl, above]{2. GET /api/run-cypress-spec (SSE)} ([yshift=-1.8cm]runner2.south);
  \draw[arw, purpleC] ([yshift=-2.6cm]runner2.south) -- node[lbl, above]{3. spawn('npx cypress run')} ([yshift=-2.6cm]cypress2.south);

  % ── SSE Loop ──
  \node[rectangle, rounded corners=2pt, draw=grayC, fill=graylight, inner sep=3pt] at ([yshift=-4cm]$(runner2.south)!0.5!(cypress2.south)$) {\footnotesize\textbf{Boucle SSE}};
  \draw[arw, orangeC] ([yshift=-4.8cm]cypress2.south) -- node[lbl, above]{4. stdout/stderr} ([yshift=-4.8cm]runner2.south);
  \draw[arw, accent] ([yshift=-5.4cm]runner2.south) -- node[lbl, above]{5. event: data (SSE)} ([yshift=-5.4cm]dash2.south);
  \draw[arw, blueC, dotted] ([yshift=-6cm]dash2.south) -- ++(0,-0.5) node[lbl, right]{\footnotesize parseCypressLogs() $\rightarrow$ rapport visuel};

  % ── End ──
  \draw[arw, orangeC, dashed] ([yshift=-7.5cm]cypress2.south) -- node[lbl, above]{6. exit code} ([yshift=-7.5cm]runner2.south);
  \draw[arw, accent] ([yshift=-8.2cm]runner2.south) -- node[lbl, above]{7. event: end} ([yshift=-8.2cm]dash2.south);
  \draw[arw, accent, dashed] ([yshift=-9cm]dash2.south) -- node[lbl, above]{8. Résumé final} ([yshift=-9cm]user2.south);
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_5.tex ======
\begin{tikzpicture}
  \node[ndGreen, minimum width=2.5cm] (vite) {\textbf{Vite Dev Server}};
  \node[ndPurple, minimum width=2.5cm, right=3cm of vite] (catalog) {\textbf{catalog.mjs}};
  \node[ndOrange, minimum width=2.5cm, right=3cm of catalog] (fsys) {\textbf{File System}};
  \node[ndBlue, minimum width=2.5cm, right=3cm of fsys] (output) {\textbf{JSON Output}};

  \foreach \n in {vite,catalog,fsys,output}
    \draw[dashed, gray!50] (\n.south) -- ++(0,-10);

  \draw[arw, accent] ([yshift=-1cm]vite.south) -- node[lbl, above]{1. npm run dev} ([yshift=-1cm]catalog.south);
  \draw[arw, purpleC] ([yshift=-2cm]catalog.south) -- node[lbl, above]{2. glob('cypress/e2e/**/*.cy.js')} ([yshift=-2cm]fsys.south);
  \draw[arw, orangeC, dashed] ([yshift=-2.8cm]fsys.south) -- node[lbl, above]{3. liste des specs} ([yshift=-2.8cm]catalog.south);
  \draw[arw, purpleC] ([yshift=-3.8cm]catalog.south) -- node[lbl, above]{4. readFile(fixtures)} ([yshift=-3.8cm]fsys.south);
  \draw[arw, orangeC, dashed] ([yshift=-4.6cm]fsys.south) -- node[lbl, above]{5. JSON fixtures} ([yshift=-4.6cm]catalog.south);

  \draw[arw, purpleC, dotted] ([yshift=-5.6cm]catalog.south) -- ++(0,-0.5) node[lbl, right]{\footnotesize inferTemplateFromFixture()};

  \draw[arw, blueC] ([yshift=-7cm]catalog.south) -- node[lbl, above]{6. writeFileSync} ([yshift=-7cm]output.south);
  \node[snote] at ([yshift=-8cm]output.south) {\footnotesize projectCatalog.json\\+ generatedFormTemplates.json};
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_6.tex ======
\begin{tikzpicture}[node distance=0.8cm and 1.5cm]
  % ── Screen ──
  \node[cls, text width=4.8cm] (screen) {
    \textbf{\textcolor{blueC}{Screen}}\\
    \rule{\linewidth}{0.4pt}\\
    - id: string\\
    - label: string\\
    - moduleName: string\\
    - workspace: string\\
    - specPath: string\\
    - fixturePath: string\\
    \rule{\linewidth}{0.4pt}\\
    + getFullPath(): string\\
    + getModule(): Module
  };

  % ── FormTemplate ──
  \node[cls, text width=4.8cm, right=1.5cm of screen] (template) {
    \textbf{\textcolor{blueC}{FormTemplate}}\\
    \rule{\linewidth}{0.4pt}\\
    - screenId: string\\
    - fields: FormField[]\\
    - predefinedCases: object[]\\
    \rule{\linewidth}{0.4pt}\\
    + getField(key): FormField\\
    + loadPredefined(name): void
  };

  % ── FormField ──
  \node[cls, text width=4.8cm, right=1.5cm of template] (field) {
    \textbf{\textcolor{blueC}{FormField}}\\
    \rule{\linewidth}{0.4pt}\\
    - key: string\\
    - label: string\\
    - type: enum\\
    - required: boolean\\
    - defaultValue: any\\
    \rule{\linewidth}{0.4pt}\\
    + validate(): boolean
  };

  % ── GenerationEngine ──
  \node[cls, text width=4.8cm, below=1.5cm of screen] (engine) {
    \textbf{\textcolor{accent}{GenerationEngine}}\\
    \rule{\linewidth}{0.4pt}\\
    - screen: Screen\\
    - formData: object\\
    - specSource: string\\
    \rule{\linewidth}{0.4pt}\\
    + buildPayload(): JSON\\
    + patchSpec(): string\\
    + generateOutput(): Output
  };

  % ── ExportAPI ──
  \node[cls, text width=4.8cm, below=1.5cm of template] (exportapi) {
    \textbf{\textcolor{accent}{ExportAPI}}\\
    \rule{\linewidth}{0.4pt}\\
    - port: number\\
    - projectRoot: string\\
    \rule{\linewidth}{0.4pt}\\
    + exportToProject(payload): void\\
    + getSpecSource(path): string\\
    + runCypressSpec(spec): SSE\\
    + getReferenciel(): JSON\\
    + getTestCard(): CardData
  };

  % ── CatalogGenerator ──
  \node[cls, text width=4.8cm, below=1.5cm of field] (catgen) {
    \textbf{\textcolor{purpleC}{CatalogGenerator}}\\
    \rule{\linewidth}{0.4pt}\\
    - specsDir: string\\
    - fixturesDir: string\\
    \rule{\linewidth}{0.4pt}\\
    + scanSpecs(): Screen[]\\
    + inferTemplates(): Template[]\\
    + writeCatalog(): void
  };

  % ── CardData ──
  \node[cls, text width=4.8cm, below=1.5cm of engine] (card) {
    \textbf{\textcolor{orangeC}{CardData}}\\
    \rule{\linewidth}{0.4pt}\\
    - cardNumber: string\\
    - expiryDate: string\\
    - clientCode: string\\
    \rule{\linewidth}{0.4pt}\\
    + format(): string
  };

  % ── TestResult ──
  \node[cls, text width=4.8cm, below=1.5cm of exportapi] (result) {
    \textbf{\textcolor{orangeC}{TestResult}}\\
    \rule{\linewidth}{0.4pt}\\
    - specName: string\\
    - passed: number\\
    - failed: number\\
    - duration: number\\
    - cases: TestCase[]
  };

  % ── TestCase ──
  \node[cls, text width=4.8cm, below=1.5cm of catgen] (testcase) {
    \textbf{\textcolor{orangeC}{TestCase}}\\
    \rule{\linewidth}{0.4pt}\\
    - title: string\\
    - status: enum\\
    - error: string | null\\
    - duration: number
  };

  % ── Relationships ──
  \draw[arw, blueC] (screen) -- node[lbl]{1..*} (template);
  \draw[arw, blueC] (template) -- node[lbl]{1..*} (field);
  \draw[arw, accent] (engine) -- node[lbl]{uses} (screen);
  \draw[arw, accent] (engine) -- (template);
  \draw[arw, accent] (exportapi) -- node[lbl]{uses} (engine);
  \draw[arw, purpleC] (catgen) -- node[lbl]{creates} (screen);
  \draw[arw, purpleC] (catgen) -- node[lbl]{creates} (template);
  \draw[arw, orangeC] (exportapi) -- node[lbl]{returns} (card);
  \draw[arw, orangeC] (exportapi) -- node[lbl]{produces} (result);
  \draw[arw, orangeC] (result) -- node[lbl]{1..*} (testcase);
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_7.tex ======
\begin{tikzpicture}[node distance=1cm and 1.5cm]
  % ── Client ──
  \node[ndBlue, minimum width=3.5cm, minimum height=1.3cm] (client) {\textbf{Dashboard React}\\\footnotesize SPA (localhost:5175)};

  % ── Gateway ──
  \node[ndGreen, minimum width=3.5cm, minimum height=1.3cm, below=1.5cm of client] (gw) {\textbf{API Gateway}\\\footnotesize localhost:8000};

  % ── Services ──
  \node[ndPurple, minimum width=2.8cm, below left=2cm and 3cm of gw] (ref) {\textbf{Referential}\\\footnotesize :8001};
  \node[ndPurple, minimum width=2.8cm, below left=2cm and 0cm of gw] (gen) {\textbf{Generator}\\\footnotesize :8002};
  \node[ndPurple, minimum width=2.8cm, below right=2cm and 0cm of gw] (run) {\textbf{Runner}\\\footnotesize :8003};
  \node[ndPurple, minimum width=2.8cm, below right=2cm and 3cm of gw] (conn) {\textbf{Connect}\\\footnotesize :8004};

  % ── Storage ──
  \node[ndGray, minimum width=3cm, below=2cm of $(gen)!0.5!(run)$] (storage) {\textbf{Stockage Partagé}\\\footnotesize cypress/ (R/W)};

  % ── External ──
  \node[ndOrange, minimum width=3cm, below=1.5cm of conn] (pwc) {\textbf{PowerCard API}\\\footnotesize Connect + App};

  % ── Arrows ──
  \draw[arw] (client) -- node[lbl]{Vite Proxy} (gw);
  \draw[arw] (gw) -- node[lbl, left]{\footnotesize /api/referenciel} (ref);
  \draw[arw] (gw) -- node[lbl, left]{\footnotesize /api/export} (gen);
  \draw[arw] (gw) -- node[lbl, right]{\footnotesize /api/run (SSE)} (run);
  \draw[arw] (gw) -- node[lbl, right]{\footnotesize /api/connect} (conn);
  \draw[arw] (ref) -- (storage);
  \draw[arw] (gen) -- (storage);
  \draw[arw] (run) -- (storage);
  \draw[arw] (conn) -- node[lbl]{JWT Auth} (pwc);
\end{tikzpicture}

====== dashboard/report/diagrams_src\diagram_8.tex ======
\begin{tikzpicture}[node distance=0.6cm and 1cm]
  % ── Entry Point ──
  \node[ndGray, minimum width=2.5cm] (html) {index.html};
  \node[ndGray, minimum width=2.5cm, right=0.5cm of html] (main) {main.jsx};
  \node[ndBlue, minimum width=3cm, minimum height=1.2cm, right=0.5cm of main] (app) {\textbf{App.jsx}\\\footnotesize 1585 lignes};

  \begin{scope}[on background layer]
    \node[bxGray, fit=(html)(main)(app), label={[lGr]above:Point d'Entrée}] {};
  \end{scope}

  % ── Data Layer ──
  \node[ndGreen, minimum width=2.5cm, below=2.5cm of html] (screens) {screens.js};
  \node[ndGreen, minimum width=2.8cm, right=0.5cm of screens] (templates) {formTemplates.js\\\footnotesize 15 templates};
  \node[ndGreen, minimum width=3cm, right=0.5cm of templates] (catalog) {projectCatalog.json\\\footnotesize 32 écrans};
  \node[ndGreen, minimum width=3cm, right=0.5cm of catalog] (gentemp) {generatedForm\\Templates.json};

  \begin{scope}[on background layer]
    \node[bxGreen, fit=(screens)(templates)(catalog)(gentemp), label={[lG]above:Couche Données}] {};
  \end{scope}

  % ── Services Layer ──
  \node[ndPurple, minimum width=3cm, below=2.5cm of screens] (connectsvc) {connectApi.js};
  \node[ndPurple, minimum width=3cm, right=0.5cm of connectsvc] (mistral) {mistralApi.js\\\footnotesize AI Gen};
  \node[ndPurple, minimum width=3cm, right=0.5cm of mistral] (aidiscovery) {aiDiscovery.js\\\footnotesize AI Crawl};

  \begin{scope}[on background layer]
    \node[bxPurple, fit=(connectsvc)(mistral)(aidiscovery), label={[lP]above:Couche Services}] {};
  \end{scope}

  % ── Server Layer ──
  \node[ndOrange, minimum width=3.5cm, below=2.5cm of connectsvc] (exportapi) {exportApi.js\\\footnotesize 5 endpoints};
  \node[ndOrange, minimum width=3.5cm, right=0.8cm of exportapi] (catalogmjs) {catalog.mjs\\\footnotesize 583 lignes};
  \node[ndOrange, minimum width=3.5cm, right=0.8cm of catalogmjs] (viteconf) {vite.config.js};

  \begin{scope}[on background layer]
    \node[bxOrange, fit=(exportapi)(catalogmjs)(viteconf), label={[lO]above:Couche Serveur (Build + API)}] {};
  \end{scope}

  % ── Arrows ──
  \draw[arw] (html) -- (main);
  \draw[arw] (main) -- (app);
  \draw[arw] (app) -- (screens);
  \draw[arw] (app) -- (templates);
  \draw[arw] (app) -- (catalog);
  \draw[arw] (app) -- (connectsvc);
  \draw[arw] (app) -- (mistral);
  \draw[arw] (catalogmjs) -- (catalog);
  \draw[arw] (catalogmjs) -- (gentemp);
  \draw[arw] (viteconf) -- (exportapi);
\end{tikzpicture}

