# NFSe Report Automation

Conjunto de scripts em Python desenvolvidos para automatizar a rotina de download de notas fiscais de serviço eletrônicas (NFS-e Nacional) e a geração de relatórios em planilha.

---

## Sobre o projeto

O NFSe Report Automation reúne três scripts que eliminam o trabalho manual de baixar notas no portal e preencher a planilha de conferência.

Os scripts consultam a API oficial do ADN (NFS-e Nacional) para baixar os XMLs das notas emitidas em um período, automatizam o navegador para baixar os PDFs e geram o relatório preenchendo uma planilha modelo em Excel.

Todo o processamento acontece localmente na máquina do usuário, sem backend próprio.

---

# Funcionalidades

## BAIXAR-XMLS (baixar_xmls_api.py)
- Consulta das notas emitidas por um CNPJ em um período informado (dd/mm/aaaa)
- Autenticação mTLS com certificado digital A1 (.pfx)
- Paginação automática da listagem de notas
- Download do XML de cada nota pela chave de acesso
- Identificação de notas canceladas com o prefixo `CANCELADA_`
- Organização dos arquivos por período na pasta `downloads/`

## BAIXAR-PDFS (main_baixar_pdfs.py)
- Download dos PDFs das notas com automação de navegador (Playwright)
- Compatível com execução como script ou como executável (.exe)

## GERAR-RELATORIO (main_gerar_relatorio.py)
- Preenchimento automático da planilha modelo com os dados das notas
- Geração do relatório pronto para conferência
- Compatível com execução como script ou como executável (.exe)

---

# Tecnologias utilizadas

## Linguagem e bibliotecas
- Python
- Playwright
- openpyxl
- requests
- cryptography
- PyAutoGUI

## Recursos utilizados
- API oficial do ADN (NFS-e Nacional) e da Sefin Nacional
- Autenticação mTLS com certificado A1
- Leitura e geração de arquivos XML e XLSX
- Variáveis de ambiente
- PyInstaller para geração de executável

---

# Estrutura do projeto

```bash
NFSe-Report-Automation/
├── baixar_xmls_api.py
├── main_baixar_pdfs.py
├── main_gerar_relatorio.py
├── requirements.txt
├── modelo/
├── src/
├── test/
└── downloads/        # gerado automaticamente, ignorado pelo git
```

---

# Instalação

## Clone o repositório

```bash
git clone https://github.com/giulialimaa7/NFSe-Report-Automation.git
cd NFSe-Report-Automation
```

---

## Instale as dependências

```bash
python -m venv venv
venv\Scripts\activate

pip install -r requirements.txt
playwright install chromium
```

---

## Configure as credenciais

As credenciais não ficam no código. Defina as variáveis de ambiente antes de executar:

| Variável | Descrição |
|---|---|
| `NFSE_PFX_PATH` | Caminho completo do certificado A1 (`.pfx`) |
| `NFSE_PFX_PASSWORD` | Senha do certificado |
| `NFSE_CNPJ` | CNPJ do prestador, somente números |

No PowerShell:

```powershell
$env:NFSE_PFX_PATH = "C:\caminho\para\certificado.pfx"
$env:NFSE_PFX_PASSWORD = "sua-senha"
$env:NFSE_CNPJ = "00000000000000"
```

---

## Gere o executável (opcional)

```bash
pip install pyinstaller
pyinstaller --onefile --name baixar_pdfs main_baixar_pdfs.py
pyinstaller --onefile --name gerar_relatorio main_gerar_relatorio.py
```

Os executáveis ficam em `dist/`. Copie junto a pasta `modelo/` e a pasta `.playwright-browsers/`.

---

# Fluxo de uso

1. O usuário executa `baixar_xmls_api.py` informando a data inicial e a final
2. O script lista as notas do período e baixa o XML de cada uma em `downloads/<periodo>/xmls/`
3. Notas canceladas são identificadas pelo prefixo `CANCELADA_`
4. O usuário executa `main_baixar_pdfs.py` para baixar os PDFs
5. O usuário executa `main_gerar_relatorio.py` para preencher a planilha modelo
6. O relatório gerado é conferido pelo usuário

```bash
python baixar_xmls_api.py 01/06/2026 09/06/2026
python main_baixar_pdfs.py
python main_gerar_relatorio.py
```

---

# Privacidade e segurança

- Os dados são processados localmente e nenhuma informação é enviada para servidor próprio
- O certificado digital, a senha e o CNPJ não ficam no código: são lidos de variáveis de ambiente
- Nunca versione o `.pfx`, senhas ou arquivos `.env`
- Os XMLs, PDFs e relatórios gerados contêm dados fiscais e pessoais. Não os versione nem os compartilhe (a pasta `downloads/` está no `.gitignore`)
- Utilize os scripts apenas com certificados e CNPJs que você tem autorização para operar

---

# Diferenciais do projeto

- Automação de uma rotina fiscal manual e repetitiva
- Uso da API oficial da NFS-e Nacional com autenticação por certificado
- Paginação e tratamento de notas canceladas
- Execução como script ou como executável (.exe)
- Projeto voltado a uma rotina real de trabalho

---

# Melhorias futuras

- Remover o limite de 10 notas usado em modo de teste no download dos XMLs
- Incluir `requests` e `cryptography` no
