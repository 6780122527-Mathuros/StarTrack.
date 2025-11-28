<!DOCTYPE html>
<html lang="th">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>StarTrack DEMO</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link
      href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap"
      rel="stylesheet"
    />

    <style>
      body {
        font-family: "Sarabun", Arial, sans-serif;
        background: linear-gradient(135deg, #f4eaff, #d3ecfd);
        color: #444;
        margin: 0;
        min-height: 100vh;
      }
      header {
        text-align: center;
        background: #fcecfb;
        border-bottom: 2px solid #e5d9f7;
        padding-top: 1.7em;
        padding-bottom: 0.3em;
      }
      h1 {
        color: #a645ae;
        margin: 0.5em 0 0.1em 0;
        font-size: 2em;
        font-weight: bold;
      }
      nav {
        text-align: center;
        padding: 1.1em;
        background: #f2f7fd;
      }
    </style>

    <!-- ✅ GitHub-compatible React CDN -->
    <script src="https://unpkg.com/react@19/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@19/umd/react-dom.production.min.js"></script>

    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide-react@latest/dist/lucide-react.min.js"></script>

    <!-- Recharts CDN -->
    <script src="https://unpkg.com/recharts/umd/Recharts.min.js"></script>

    <!-- (Optional) Google GenAI – ใช้แบบ ESM ผ่าน JSDelivr -->
    <script type="module">
      import { GoogleGenerativeAI } from "https://cdn.jsdelivr.net/npm/@google/generative-ai/+esm";
      window.GoogleGenerativeAI = GoogleGenerativeAI;
    </script>

    <!-- React App -->
    <script type="module">
      const { useState, useEffect } = React;
      const root = ReactDOM.createRoot(document.getElementById("root"));

      function App() {
        return React.createElement(
          "div",
          { className: "p-6 text-center text-xl" },
          "🚀 StarTrack DEMO is running on GitHub Pages!"
        );
      }

      root.render(React.createElement(App));
    </script>
  </head>

  <body>
    <div id="root"></div>
  </body>
</html>
