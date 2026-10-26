# 📊 Automação de Processo – Indicadores de Lojas

Projeto em **Python + Pandas** que automatiza a consolidação das vendas de uma rede de lojas (shoppings), organizando os dados por loja, gerando backups em Excel e calculando indicadores e rankings de faturamento.

O objetivo é substituir um processo manual e repetitivo por um script que, a partir das bases de dados da empresa, entrega em poucos segundos os arquivos e métricas de cada loja.

---

## 🎯 O que o projeto faz

1. **Importa as bases de dados** (vendas, lojas e e-mails dos gerentes).
2. **Une as vendas com as lojas** pelo `ID Loja`, associando cada venda ao nome da loja.
3. **Separa as vendas por loja**, criando um dicionário `{loja: DataFrame}`.
4. **Identifica o dia do indicador** (data mais recente da base de vendas).
5. **Cria backups em Excel**: uma pasta para cada loja, com o arquivo `mês_dia_Loja.xlsx`.
6. **Calcula os indicadores** de cada loja (anual e do dia):
   - Faturamento
   - Diversidade de produtos (quantidade de produtos únicos vendidos)
   - Ticket médio por venda
7. **Gera os rankings de faturamento** das lojas (anual e do dia) e salva em Excel.

---

## 📁 Estrutura de pastas

```
.
├── Automacao_de_Processo.ipynb     # Notebook principal
├── Bases de Dados/
│   ├── Emails.xlsx                 # Lojas e gerentes responsáveis
│   ├── Lojas.csv                   # ID Loja e nome da loja (sep=';', encoding latin1)
│   └── Vendas.xlsx                 # Histórico de vendas
└── Backup Arquivos Lojas/          # Gerada/preenchida pelo script
    ├── <Nome da Loja>/
    │   └── <mês>_<dia>_<Nome da Loja>.xlsx
    ├── <mês>_<dia>_Ranking Anual.xlsx
    └── <mês>_<dia>_Ranking Dia.xlsx
```

> ⚠️ A pasta `Backup Arquivos Lojas` precisa existir antes da execução. As subpastas de cada loja são criadas automaticamente.

---

## 🗃️ Bases de dados

| Arquivo | Conteúdo |
|---|---|
| `Vendas.xlsx` | Código da venda, data, ID da loja, produto, quantidade, valor final, entre outros |
| `Lojas.csv` | Relação entre `ID Loja` e `Loja` |
| `Emails.xlsx` | Loja, gerente responsável e contato |

---

## 🎯 Metas utilizadas

As metas estão definidas no notebook e podem ser ajustadas conforme a necessidade:

| Indicador | Meta do dia | Meta do ano |
|---|---|---|
| Faturamento | R$ 1.000 | R$ 1.650.000 |
| Diversidade de produtos | 4 | 120 |
| Ticket médio | R$ 500 | R$ 500 |

---

## 🛠️ Tecnologias

- Python 3
- [pandas](https://pandas.pydata.org/)
- [openpyxl](https://openpyxl.readthedocs.io/) (leitura/escrita de `.xlsx`)
- `pathlib` (manipulação de pastas e arquivos)
- Jupyter Notebook

---

## ▶️ Como executar

1. Clone o repositório:
   ```bash
   git clone <url-do-repositorio>
   cd <nome-do-repositorio>
   ```

2. Instale as dependências:
   ```bash
   pip install pandas openpyxl jupyter
   ```

3. Garanta que as pastas `Bases de Dados` e `Backup Arquivos Lojas` existam na mesma pasta do notebook, com os arquivos de dados no lugar certo.

4. Abra e execute o notebook:
   ```bash
   jupyter notebook Automacao_de_Processo.ipynb
   ```

> 💡 Os caminhos no notebook usam `\` (padrão Windows). No Linux/macOS, substitua por `/` ou use `pathlib.Path('Bases de Dados') / 'Vendas.xlsx'`.

---

## 📤 Saídas geradas

- **Backup por loja:** `Backup Arquivos Lojas/<Loja>/<mês>_<dia>_<Loja>.xlsx`
- **Ranking anual de faturamento:** `Backup Arquivos Lojas/<mês>_<dia>_Ranking Anual.xlsx`
- **Ranking do dia:** `Backup Arquivos Lojas/<mês>_<dia>_Ranking Dia.xlsx`

---

## 🚧 Melhorias futuras

- [ ] Comparar os indicadores com as metas (ex.: ✅ / ❌ ou cores verde/vermelho)
- [ ] Enviar por e-mail o relatório de cada loja ao respectivo gerente (a base `Emails.xlsx` já é carregada)
- [ ] Enviar um e-mail de resumo com os rankings para a diretoria
- [ ] Organizar o código em funções e/ou scripts `.py`
- [ ] Agendar a execução automática diária

---

## 👤 Autor

Desenvolvido por **<seu nome>** – [LinkedIn](https://linkedin.com/in/seu-perfil) · [GitHub](https://github.com/seu-usuario)
