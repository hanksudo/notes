# Vercel CLI

## Installation

```zsh
npm install -g vercel
```

## Backup

```zsh
vercel env pull
```

## Add environment variables

```zsh
cat .env.production | sed 's/=/ /' | xargs -n 2 bash -c 'echo -n $1 | vercel env add $0 production'
cat .env.staging | sed 's/=/ /' | xargs -n 2 bash -c 'echo -n $1 | vercel env add $0 development'
cat .env.staging | sed 's/=/ /' | xargs -n 2 bash -c 'echo -n $1 | vercel env add $0 preview'
```

## Remove all Vercel environment variables

```zsh
vercel env ls | sed 's/=/ /' | awk 'NR>2 {print $1, $3}' | xargs -n 2 bash -c 'vercel env rm $0 ${1,,} -y'
```
