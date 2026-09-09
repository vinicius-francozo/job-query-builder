# Job Query Builder

> Montador de buscas booleanas para caçar vagas no LinkedIn e no Google.

Procurar vaga em site de emprego é ruim: o filtro é raso, a busca ignora sinônimos
e metade dos resultados é anúncio de consultoria de recrutamento. Quem sabe montar
uma **query booleana** encontra em uma busca o que o filtro nativo não entrega em dez.

O problema é que ninguém quer decorar sintaxe de operador. Este projeto resolve isso:
você preenche um formulário, ele monta a query e te dá o link da busca pronto.

```
[ Cargo    ] backend developer
[ Skills   ] node, typescript          (OR)
[ Local    ] São Paulo
[ Excluir  ] recruiter, estágio
[ Fonte    ] (•) Google X-ray   ( ) LinkedIn Jobs

  ↓

site:linkedin.com/jobs ("backend developer")
("node" OR "typescript") "São Paulo"
-recruiter -estágio

[ Copiar ]   [ Abrir busca ↗ ]
```

## O que ele faz

- **Formulário → query.** Cargo, skills, localidade, senioridade, modelo de trabalho
  e termos a excluir viram uma string booleana correta, com aspas, parênteses e
  precedência de `OR`/`AND` no lugar certo.
- **Multi-alvo.** A mesma intenção de busca é compilada para alvos diferentes:
  Google X-ray (`site:`, `intitle:`, `inurl:`), LinkedIn Jobs, Google Jobs.
  Cada alvo tem suas próprias regras — o LinkedIn, por exemplo, não aceita `site:`.
- **Sinônimos e variações.** `backend` também procura `back-end`, `back end`,
  `backend developer`, `desenvolvedor backend`. É onde a busca manual costuma falhar.
- **Presets.** Perfis salvos ("dev backend remoto PJ", "product designer sênior")
  para reusar a busca toda semana sem remontar.
- **Link direto.** Botão que abre a busca já executada, não só a string copiada.
- **Sem backend.** Roda inteiro no navegador, nada de credencial ou scraping.

## Por que não é scraping

Este projeto **não** raspa o LinkedIn nem o Google. Ele monta a URL de busca que
você mesmo abriria no navegador e usa a busca pública do próprio site. Nada de
login, token, automação de sessão ou termos de uso violados.

## Stack

- [Vite](https://vitejs.dev/) + [React](https://react.dev/) + TypeScript
- Estado da query em um modelo tipado, com compiladores por alvo (`google`, `linkedin`)
- Deploy estático via GitHub Pages

## Arquitetura pretendida

```
src/
  core/
    query.ts          # o modelo tipado da busca (fonte da verdade)
    synonyms.ts       # expansão de termos e variações de grafia
    targets/
      google.ts       # QueryModel -> string + URL do Google
      linkedin.ts     # QueryModel -> string + URL do LinkedIn Jobs
  ui/
    QueryForm.tsx
    QueryPreview.tsx
    PresetList.tsx
```

A ideia é manter `core/` sem nenhuma dependência de React — assim os compiladores
podem ser testados isoladamente e, mais tarde, virar uma CLI ou extensão de browser.

## Roadmap

- [ ] Modelo tipado da query + compilador para Google X-ray
- [ ] Compilador para LinkedIn Jobs
- [ ] UI do formulário com preview ao vivo da query
- [ ] Dicionário de sinônimos para cargos e skills de tecnologia
- [ ] Presets persistidos em `localStorage`
- [ ] Testes dos compiladores (casos de escape, aspas, precedência)
- [ ] Deploy no GitHub Pages
- [ ] Extra: exportar a busca como script Power Query M para Excel/Power BI

## Rodando local

> Ainda não implementado — o scaffold entra no primeiro commit de código.

```bash
npm install
npm run dev
```

## Licença

[MIT](LICENSE)
