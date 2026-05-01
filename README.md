# Plataforma de Gestão de Rotas — Logística

Sistema de gestão de rotas dinâmicas para operação logística. Permite criar, monitorar e fechar rotas de entrega com suporte a Milk Run, cálculo automático de rentabilidade e controle de motoristas e sellers.

![Dashboard](https://img.shields.io/badge/status-demo-10b981) ![Stack](https://img.shields.io/badge/stack-Tailwind%20%7C%20Chart.js%20%7C%20Supabase-0f172a)

## Funcionalidades

- **Criação de rotas** — rota simples ou Milk Run (múltiplos stops em uma saída)
- **Gestão de motoristas** — cadastro com tipo de veículo, placa e flag de aprovação especial
- **Gestão de sellers** — cadastro por DOP e SOC com integração nas rotas
- **Rentabilidade automática** — cálculo em tempo real de frete, receita e % de custo por rota
- **Analytics** — gráficos de volume, receita e rentabilidade por período
- **Controle de status** — rotas Abertas e Fechadas com histórico
- **Taxas por região** — SP Capital, RJ e Interior com taxas diferenciadas

## Stack

| Tecnologia | Uso |
|-----------|-----|
| Tailwind CSS | Estilização via CDN |
| Chart.js 4 | Gráficos de analytics |
| Supabase | Banco de dados, Auth e API REST |
| Vanilla JS (ES6+) | Lógica de negócio |

## Modo Demo

Este repositório contém uma versão com **dados fictícios em memória** — o login é automático e todas as operações (criar, editar, excluir rotas) funcionam localmente sem backend.

Para conectar ao seu Supabase, edite as variáveis no `index.html`:

```js
const SUPABASE_URL = "https://seu-projeto.supabase.co";
const SUPABASE_ANON_KEY = "sua-chave-anon";
```

### Schema Supabase

```sql
CREATE TABLE drivers (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  tipo_veiculo TEXT,
  placa TEXT,
  legacy_id TEXT,
  precisa_justificativa BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE sellers (
  id SERIAL PRIMARY KEY,
  dop TEXT NOT NULL,
  nome TEXT NOT NULL,
  soc TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE saidas (
  id SERIAL PRIMARY KEY,
  data DATE,
  operacao TEXT,
  soc TEXT,
  motorista_id INT REFERENCES drivers(id),
  tipo_veiculo TEXT,
  is_milk_run BOOLEAN DEFAULT FALSE,
  status TEXT DEFAULT 'ABERTO',
  frete_total NUMERIC,
  receita_estimada NUMERIC,
  rentabilidade_pct NUMERIC,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE saida_stops (
  id SERIAL PRIMARY KEY,
  saida_id INT REFERENCES saidas(id),
  ordem INT,
  seller_dop TEXT,
  rota_id TEXT,
  rota_nome TEXT,
  sacas INT DEFAULT 0,
  caixas INT DEFAULT 0,
  volume_total INT DEFAULT 0,
  frete_rateado NUMERIC,
  receita_estimada NUMERIC,
  rentabilidade_pct NUMERIC
);
```

## Como rodar

```bash
# Abrir direto no browser (modo demo, sem configuração)
open index.html

# Ou via servidor local
npx serve .
python -m http.server 8080
```

## Projeto

Desenvolvido para operação logística que precisava controlar diariamente saídas de motoristas com múltiplas entregas. O sistema automatiza o cálculo de rentabilidade por rota e centraliza o histórico de operações.

**Segmento:** Logística e distribuição  
**Diferencial:** Suporte a Milk Run com rateio automático de frete entre stops

---

*Portfólio — [SalvianoLopes](https://github.com/SalvianoLopes)*
