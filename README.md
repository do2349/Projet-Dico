# 🎯 StoryDico - Guide de Déploiement Complet

Application pour suivre et compter tes centres d'intérêt créatifs.

## 📋 Table des Matières

1. [Prérequis](#prérequis)
2. [Configuration Supabase](#configuration-supabase)
3. [Configuration Locale](#configuration-locale)
4. [Déploiement sur Vercel](#déploiement-sur-vercel)
5. [Utilisation](#utilisation)

---

## 🔧 Prérequis

Tu auras besoin de :
- Un compte [Supabase](https://supabase.com) (gratuit)
- Un compte [Vercel](https://vercel.com) (gratuit)
- Git installé sur ton ordinateur

---

## 🗄️ Configuration Supabase

### Étape 1 : Créer un Projet Supabase

1. Va sur [supabase.com](https://supabase.com)
2. Clique sur **"Start your project"** ou **"New Project"**
3. Choisis un nom pour ton projet (ex: `storydico`)
4. Choisis une région (prends **Europe West** si tu es en France)
5. Crée un mot de passe pour la base de données (garde-le précieusement)
6. Clique sur **"Create new project"**
7. ⏳ Attends 2-3 minutes que le projet soit créé

### Étape 2 : Créer la Table de Données

Une fois ton projet créé :

1. Dans le menu de gauche, clique sur **"SQL Editor"**
2. Clique sur **"New query"**
3. Colle ce code SQL :

```sql
-- Create words table
create table public.words (
  id uuid default gen_random_uuid() primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  text text not null,
  count integer default 0 not null,
  user_id uuid references auth.users not null
);

-- Enable Row Level Security (RLS)
alter table public.words enable row level security;

-- Create policy: Users can only see their own words
create policy "Users can view their own words"
  on public.words for select
  using (auth.uid() = user_id);

-- Create policy: Users can insert their own words
create policy "Users can insert their own words"
  on public.words for insert
  with check (auth.uid() = user_id);

-- Create policy: Users can update their own words
create policy "Users can update their own words"
  on public.words for update
  using (auth.uid() = user_id);

-- Create policy: Users can delete their own words
create policy "Users can delete their own words"
  on public.words for delete
  using (auth.uid() = user_id);
```

4. Clique sur **"Run"** (en bas à droite)
5. ✅ Tu devrais voir "Success. No rows returned"

### Étape 3 : Récupérer les Clés API

1. Dans le menu de gauche, clique sur **"Settings"** (icône engrenage)
2. Clique sur **"API"**
3. Tu verras deux informations importantes :
   - **Project URL** (commence par `https://`)
   - **anon public** key (longue clé qui commence par `eyJ...`)
4. 📝 **Copie ces deux valeurs**, on en aura besoin !

### Étape 4 : Configurer l'Authentification Google (Optionnel)

Si tu veux permettre la connexion avec Google :

1. Dans le menu de gauche, clique sur **"Authentication"**
2. Clique sur **"Providers"**
3. Active **"Google"**
4. Suis les instructions pour obtenir les Client ID et Secret depuis [Google Cloud Console](https://console.cloud.google.com)

---

## 💻 Configuration Locale

### Étape 1 : Télécharger le Projet

1. Télécharge tous les fichiers du projet sur ton ordinateur
2. Place-les dans un dossier (ex: `storydico-app`)

### Étape 2 : Configurer les Variables d'Environnement

1. Dans le dossier du projet, crée un fichier nommé `.env.local`
2. Ouvre-le et ajoute :

```
NEXT_PUBLIC_SUPABASE_URL=ta_project_url_ici
NEXT_PUBLIC_SUPABASE_ANON_KEY=ta_anon_key_ici
```

3. Remplace les valeurs par celles copiées depuis Supabase
4. Sauvegarde le fichier

### Étape 3 : Tester en Local (Optionnel)

Si tu veux tester avant de déployer :

```bash
# Installe les dépendances
npm install

# Lance l'app en local
npm run dev
```

Ouvre ton navigateur sur `http://localhost:3000`

---

## 🚀 Déploiement sur Vercel

### Méthode 1 : Déploiement avec GitHub (Recommandé)

#### 1. Créer un Repo GitHub

1. Va sur [github.com](https://github.com)
2. Crée un nouveau repository (clique sur le `+` en haut à droite)
3. Nomme-le `storydico` (ou ce que tu veux)
4. Choisis **"Private"** si tu veux que ce soit privé
5. Ne coche rien d'autre, clique sur **"Create repository"**

#### 2. Uploader ton Code

Dans ton terminal (ou Git Bash sur Windows) :

```bash
# Va dans le dossier de ton projet
cd chemin/vers/storydico-app

# Initialise Git
git init

# Ajoute tous les fichiers
git add .

# Commit
git commit -m "Initial commit"

# Connecte-toi à GitHub
git remote add origin https://github.com/ton-username/storydico.git

# Push
git push -u origin main
```

#### 3. Déployer sur Vercel

1. Va sur [vercel.com](https://vercel.com)
2. Connecte-toi avec ton compte GitHub
3. Clique sur **"Add New..."** → **"Project"**
4. Sélectionne ton repository `storydico`
5. Clique sur **"Import"**

#### 4. Configurer les Variables d'Environnement

1. Dans la section **"Environment Variables"** :
   - Ajoute `NEXT_PUBLIC_SUPABASE_URL` → ta Project URL
   - Ajoute `NEXT_PUBLIC_SUPABASE_ANON_KEY` → ta anon key
2. Clique sur **"Deploy"**
3. ⏳ Attends 2-3 minutes

### Méthode 2 : Déploiement Direct (Plus Rapide)

1. Va sur [vercel.com](https://vercel.com)
2. Installe Vercel CLI : `npm install -g vercel`
3. Dans ton terminal :

```bash
cd chemin/vers/storydico-app
vercel
```

4. Suis les instructions
5. Ajoute les variables d'environnement quand demandé

---

## 🎉 C'est Terminé !

Ton app est maintenant en ligne ! 🚀

### URL de ton App

Tu recevras une URL type : `https://storydico.vercel.app`

Tu peux :
- Créer un compte avec email/mdp
- Te connecter avec Google (si configuré)
- Accéder depuis n'importe quel appareil
- Tes données sont synchronisées automatiquement

---

## 🛠️ Fonctionnalités

- ✅ Ajout de mots/concepts
- ✅ Compteur cliquable (+1 à chaque clic)
- ✅ Édition inline (clic sur le mot)
- ✅ Suppression (efface tout le texte)
- ✅ Tri automatique (par compteur, puis date)
- ✅ Synchronisation cloud
- ✅ Authentification sécurisée
- ✅ Responsive (mobile + desktop)

---

## 🆘 Besoin d'Aide ?

### Problèmes Courants

**"Error: Invalid API key"**
→ Vérifie que tes clés Supabase sont correctes dans `.env.local`

**"Table 'words' does not exist"**
→ Tu as oublié de créer la table dans Supabase (Étape 2)

**"Row Level Security policy violation"**
→ Vérifie que les policies RLS sont bien créées

---

## 📝 Notes

- Les données sont privées (chaque utilisateur voit uniquement ses propres mots)
- Gratuit jusqu'à 50k utilisateurs (Supabase + Vercel free tier)
- Pas besoin de carte bancaire

---

**Profite de ton StoryDico ! ✨**
