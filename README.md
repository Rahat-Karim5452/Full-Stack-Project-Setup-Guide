# 🚀 Full-Stack Project Setup Guide

A complete quick-reference boilerplate guide for setting up the **Server-Side (Node.js, Express, MongoDB)**, **CRUD Operations**, and **Client-Side (Next.js, shadcn/ui, Google Fonts)**.

---

## 📑 Table of Contents
- [🖥️ Server-Side Setup](#️-server-side-setup)
  - [Initial Setup & Dependencies (Steps 1–7)](#initial-setup--dependencies)
  - [Universal Server Boilerplate (Step 8)](#8-universal-server-boilerplate-same-for-all-projects)
- [🔄 Server & Client-Side CRUD Use](#-server--client-side-crud-use)
  - [1. GET Data (Read from Database)](#1-get-information-from-database)
  - [2. POST Data (Send from Client to Database)](#2-post-data-from-client-to-database)
- [💻 Client-Side Setup](#-client-side-setup)
  - [Next.js & shadcn/ui Setup (Steps 1–3)](#initial-setup-nextjs--shadcnui)
  - [Font Setup in `layout.tsx` (Step 4)](#4-font-setup-guide-layouttsx)
- [🔤 Professional Fonts Theme Preview](#-professional-fonts-theme-preview)

---

# 🖥️ Server-Side Setup

### Initial Setup & Dependencies

#### 1. Initialize Node.js Project
```bash
npm init -y
```
> **Important:** Add `"type": "module"` inside your `package.json` so you can use ES6 `import` and top-level `await`:
```json
{
  "type": "module"
}
```

#### 2. Install Express
```bash
npm install express
```
```javascript
import express from "express";
```

#### 3. Install Nodemon (Globally)
```bash
npm install -g nodemon
```

#### 4. Install Dotenv & Create `.env` File
```bash
npm install dotenv --save
```
```javascript
import "dotenv/config";
```
Create a `.env` file in your server root folder:
```env
PORT=8000
MONGODB_URI=your_mongodb_connection_string_here
```

#### 5. Create a `.gitignore` File
Create a `.gitignore` file in your server root folder and add:
```gitignore
node_modules/
.env
```

#### 6. Install MongoDB
```bash
npm install mongodb
```

#### 7. Install CORS
```bash
npm i cors
```

---

### ⚡ Quick One-Line Install (Optional)
If you want to install all local server packages at once:
```bash
npm install express dotenv mongodb cors
```

---

### 8. Universal Server Boilerplate (Same for All Projects)
Paste this standard template into your main server file (e.g., `index.js` or `server.js`):

```javascript
import { MongoClient } from "mongodb";
import express from "express";
import "dotenv/config";
import cors from "cors";

const app = express();
const PORT = process.env.PORT || 8000;

app.use(cors());
app.use(express.json());

const client = new MongoClient(process.env.MONGODB_URI);

export async function connectToMongoDB() {
  try {
    await client.connect();
    console.log("You Successfully connected to MongoDB");
    return client;
  } catch (err) {
    console.dir(err);
  }
}

const mongoClient = await connectToMongoDB();
const db = mongoClient.db();

app.get("/", (req, res) => {
  res.send({ msg: "Server is running fine!" });
});

app.listen(PORT, () => {
  console.log(`Server is running at ${PORT}`);
});
```

---

# 🔄 Server & Client-Side CRUD Use

### Database & Collection Reference (Server-Side)
Define your database and collection once after connecting to MongoDB:
```javascript
const db = mongoClient.db("wanderlustProject");
const destinationCollection = db.collection("destinations");
```

---

### 1. GET Information from Database

#### 🖥️ Server-Side (API Route to Fetch Data)
```javascript
app.get("/destinations", async (req, res) => {
  const result = await destinationCollection.find({}).toArray();
  res.send(result);
});
```

#### 💻 Client-Side (Fetch & Show Data)
```tsx
const res = await fetch(`http://localhost:8000/destinations`);
const destinations = await res.json();
```

---

### 2. POST Data from Client to Database

#### 💻 Client-Side (Form Submit Handler)
```tsx
async function handleSubmit(event: React.FormEvent<HTMLFormElement>) {
  event.preventDefault();
  const res = await fetch(`http://localhost:8000/add-destination`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(draft),
  });
  const data = await res.json();
  console.log(data);
}
```

#### 🖥️ Server-Side (API Route to Insert Data)
```javascript
const db = mongoClient.db("wanderlustProject");
const destinationCollection = db.collection("destinations");

app.post("/add-destination", async (req, res) => {
  const data = req.body;
  const result = await destinationCollection.insertOne(data);
  res.send(result);
});
```

---

# 💻 Client-Side Setup

### Initial Setup (Next.js + shadcn/ui)

#### 1. Initialize Next.js with `shadcn/ui`
```bash
npx shadcn@latest init -t next
```

#### 2. Add a Single `shadcn/ui` Component (e.g., Card)
```bash
npx shadcn@latest add card
```

#### 3. Add Multiple / All `shadcn/ui` Components
```bash
npx shadcn@latest add
```

---

### 4. Font Setup Guide (`layout.tsx`)

#### 📌 Initially Provided in `layout.tsx` (Default)
By default, Next.js sets up both Sans (`Geist`) and Mono (`Geist_Mono`) fonts:

```tsx
import { Geist, Geist_Mono } from "next/font/google"
import "./globals.css"
import { ThemeProvider } from "@/components/theme-provider"
import { cn } from "@/lib/utils"

const geist = Geist({ subsets: ["latin"], variable: "--font-sans" })
const fontMono = Geist_Mono({
  subsets: ["latin"],
  variable: "--font-mono",
})

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode
}>) {
  return (
    <html
      lang="en"
      suppressHydrationWarning
      className={cn("antialiased", fontMono.variable, "font-sans", geist.variable)}
    >
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```

#### ✅ After Changing Font (Without Using Mono Font)
Replace the default font setup in `layout.tsx` with your preferred Google Font (e.g., `Merriweather`) and remove the Mono font:

```tsx
import { Merriweather } from "next/font/google"
import "./globals.css"
import { ThemeProvider } from "@/components/theme-provider"
import { cn } from "@/lib/utils"

const merriweather = Merriweather({ subsets: ["latin"] })

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode
}>) {
  return (
    <html
      lang="en"
      suppressHydrationWarning
      className={cn(`${merriweather.className} h-full antialiased`)}
    >
      <body className="min-h-full">
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```

---

# 🔤 Professional Fonts Theme Preview

Save the code below as `font-preview.html` and open it in any browser to visually compare professional fonts (`Inter`, `Roboto`, `Open Sans`, `Lato`, `Montserrat`, `Poppins`, `Source Sans 3`, `Playfair Display`, `Merriweather`, `Lora`) for future projects:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Font Preview</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&family=Roboto:wght@400;700&family=Open+Sans:wght@400;700&family=Lato:wght@400;700&family=Montserrat:wght@400;700&family=Poppins:wght@400;700&family=Source+Sans+3:wght@400;700&family=Playfair+Display:wght@400;700&family=Merriweather:wght@400;700&family=Lora:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body { background: #f5f5f5; margin: 0; padding: 40px 20px; color: #222; }
    .card { background: #fff; max-width: 800px; margin: 0 auto 24px; padding: 24px 32px; border-radius: 8px; box-shadow: 0 1px 4px rgba(0,0,0,0.1); }
    .label { font-family: Arial, sans-serif; font-size: 12px; color: #888; text-transform: uppercase; letter-spacing: 1px; }
    h2 { font-size: 32px; margin: 8px 0; }
    p { font-size: 16px; line-height: 1.6; margin: 0; }
  </style>
</head>
<body>
  <script>
    const fonts = [
      "Inter",
      "Roboto",
      "Open Sans",
      "Lato",
      "Montserrat",
      "Poppins",
      "Source Sans 3",
      "Playfair Display",
      "Merriweather",
      "Lora"
    ];
    fonts.forEach(function (name) {
      document.body.innerHTML +=
        '<div class="card" style="font-family: \'' + name + '\'">' +
        '<div class="label">' + name + '</div>' +
        '<h2>Welcome to Our Website</h2>' +
        '<p>We help businesses grow with simple, reliable solutions. The quick brown fox jumps over the lazy dog. 0123456789</p>' +
        '</div>';
    });
  </script>
</body>
</html>
```
