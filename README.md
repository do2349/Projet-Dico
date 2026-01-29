# 🎯 StoryDico - Guide de Déploiement

Application pour suivre et compter tes centres d'intérêt créatifs.

## 🚀 Installation Rapide

### 1. Supabase (5 minutes)

1. Va sur [supabase.com](https://supabase.com) et crée un compte
2. Crée un nouveau projet (nomme-le `storydico`)
3. Va dans **SQL Editor** et exécute ce code :

```sql
create table public.words (
  id uuid default gen_random_uuid() primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  text text not null,
  count integer default 0 not null,
  user_id uuid references auth.users not null
);

alter table public.words enable row level security;

create policy "Users can view their own words"
  on public.words for select using (auth.uid() = user_id);

create policy "Users can insert their own words"
  on public.words for insert with check (auth.uid() = user_id);

create policy "Users can update their own words"
  on public.words for update using (auth.uid() = user_id);

create policy "Users can delete their own words"
  on public.words for delete using (auth.uid() = user_id);
```

4. Va dans **Settings > API** et copie :
   - Project URL
   - anon public key

### 2. Configuration Locale (2 minutes)

1. Crée un fichier `.env.local` à la racine du projet
2. Ajoute tes clés :

```
NEXT_PUBLIC_SUPABASE_URL=ta_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=ta_anon_key
```

3. Installe les dépendances :

```bash
npm install
```

4. Lance en local pour tester :

```bash
npm run dev
```

Ouvre http://localhost:3000

### 3. Déploiement Vercel (3 minutes)

**Option A - Via GitHub (recommandé) :**

1. Crée un repo sur GitHub
2. Push ton code :
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/ton-username/storydico.git
git push -u origin main
```

3. Va sur [vercel.com](https://vercel.com)
4. Importe ton repo
5. Ajoute les variables d'environnement
6. Deploy !

**Option B - Via CLI Vercel :**

```bash
npm install -g vercel
vercel
```

Suis les instructions et ajoute tes variables d'environnement.

## ✨ Fonctionnalités

- ✅ Authentification (Email/Password + Google)
- ✅ Ajout de mots/concepts
- ✅ Compteur cliquable (+1)
- ✅ Édition inline des mots
- ✅ Suppression (efface le texte)
- ✅ Tri automatique (compteur DESC, puis date DESC)
- ✅ Sync cloud automatique
- ✅ Responsive mobile

## 📱 Utilisation

1. Crée un compte ou connecte-toi
2. Ajoute des mots dans la barre en haut
3. Clique sur [+] pour incrémenter le compteur
4. Clique sur un mot pour le modifier ou le supprimer
5. Tes données sont synchronisées automatiquement

## 🆘 Problèmes Courants

**"Invalid API key"**
→ Vérifie tes clés dans `.env.local`

**"Table does not exist"**
→ Tu as oublié de créer la table dans Supabase

**"Policy violation"**
→ Les policies RLS ne sont pas bien créées

## 📝 Structure du Projet

```
storydico/
├── app/
│   ├── globals.css
│   ├── layout.js
│   └── page.js
├── components/
│   ├── Auth.js
│   └── WordList.js
├── lib/
│   └── supabase.js
├── public/
├── .env.local
├── .env.local.example
├── .gitignore
├── next.config.js
├── package.json
├── postcss.config.js
├── tailwind.config.js
└── README.md
```

## 🔒 Sécurité

- Row Level Security (RLS) activé
- Chaque utilisateur voit uniquement ses propres données
- Authentification sécurisée via Supabase

## 💰 Coûts

- **Gratuit** jusqu'à 50k utilisateurs (Supabase + Vercel free tier)
- Pas de carte bancaire requise

---

**Profite de StoryDico ! ✨**

Pour toute question, consulte la documentation officielle :
- [Next.js](https://nextjs.org/docs)
- [Supabase](https://supabase.com/docs)
- [Vercel](https://vercel.com/docs)
