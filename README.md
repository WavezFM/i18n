# WavezFM i18n

Community-maintained interface translations for [WavezFM](https://wavez.fm), a social music platform built around live rooms.

## Languages

| Flag | File | Language |
| :---: | --- | --- |
| <img src="./assets/flags/en-US.svg" width="24" alt="United States flag"> | `en-US.json` | English (United States) — source locale |
| <img src="./assets/flags/pt-BR.svg" width="24" alt="Brazil flag"> | `pt-BR.json` | Portuguese (Brazil) |
| <img src="./assets/flags/cs-CZ.svg" width="24" alt="Czechia flag"> | `cs-CZ.json` | Czech |
| <img src="./assets/flags/es-ES.svg" width="24" alt="Spain flag"> | `es-ES.json` | Spanish |
| <img src="./assets/flags/fr-FR.svg" width="24" alt="France flag"> | `fr-FR.json` | French |
| <img src="./assets/flags/ja-JP.svg" width="24" alt="Japan flag"> | `ja-JP.json` | Japanese |
| <img src="./assets/flags/ko-KR.svg" width="24" alt="South Korea flag"> | `ko-KR.json` | Korean |
| <img src="./assets/flags/pl-PL.svg" width="24" alt="Poland flag"> | `pl-PL.json` | Polish |
| <img src="./assets/flags/ru-RU.svg" width="24" alt="Russia flag"> | `ru-RU.json` | Russian |

## Contributing

1. Fork and clone this repository.
2. Find the source string in `en-US.json`.
3. Edit the matching value in the target locale. Keep its key and nesting unchanged.
4. Validate every JSON file.
5. Open a pull request describing the locale and changed sections.

Only translate values. Do not translate JSON keys, product names such as `WavezFM`, URLs, or technical identifiers.

## Message syntax

Messages may use ICU MessageFormat variables, plural rules, and selectors:

```json
{
  "welcome": "Welcome, {username}!",
  "listeners": "{count, plural, one {# listener} other {# listeners}}"
}
```

Preserve every placeholder from the English source, including braces and variable names. Add locale-specific plural categories when needed. Russian, for example, commonly uses `one`, `few`, `many`, and `other`.

Preserve escaped characters too:

- `\n` for line breaks
- `\"` for quotes inside strings
- `\\` for literal backslashes

## Validation

Run this command from repository root. It checks JSON syntax plus missing or extra keys against `en-US.json`.

```bash
node -e "const fs=require('fs');const files=fs.readdirSync('.').filter(f=>/^[a-z]{2}-[A-Z]{2}\\.json$/.test(f));const flat=(o,p='',r={})=>{for(const[k,v]of Object.entries(o)){const q=p?p+'.'+k:k;v&&typeof v==='object'&&!Array.isArray(v)?flat(v,q,r):r[q]=v}return r};const all=Object.fromEntries(files.map(f=>[f,flat(JSON.parse(fs.readFileSync(f,'utf8')))]));const base=all['en-US.json'];let failed=false;for(const f of files){const missing=Object.keys(base).filter(k=>!(k in all[f]));const extra=Object.keys(all[f]).filter(k=>!(k in base));console.log(`${f}: ${Object.keys(all[f]).length} keys, ${missing.length} missing, ${extra.length} extra`);if(missing.length||extra.length){failed=true;console.log({missing,extra})}}process.exitCode=failed?1:0"
```

Before submitting:

- JSON parses without errors.
- Target file has same keys as `en-US.json`.
- ICU placeholders match source message.
- Translation fits context, tone, and UI length.
- Unrelated translations remain untouched.

## Adding a language

Copy `en-US.json` to a locale file named with a supported BCP 47 language tag, such as `de-DE.json`. Translate every value, keep full source structure, then include new locale in your pull request description.

## Reporting translation issues

Open a GitHub issue with locale, full JSON key, current text, suggested text, and screenshot or UI context when available.