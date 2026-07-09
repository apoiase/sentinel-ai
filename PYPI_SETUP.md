# Configuração de Publicação no PyPI

O pacote se chama `sentinel-guardrails` no PyPI (já que `sentinel-ai` já estava em uso).

## Configuração única: Configurar Trusted Publishing

1. Acesse https://pypi.org/manage/account/publishing/
2. Clique em "Add a new pending publisher"
3. Preencha:
   - **PyPI Project Name:** `sentinel-guardrails`
   - **Owner:** `MaxwellCalkin`
   - **Repository name:** `sentinel-ai`
   - **Workflow name:** `publish.yml`
   - **Environment name:** `pypi`
4. Clique em "Add"

Adicione também para o TestPyPI em https://test.pypi.org/manage/account/publishing/:
   - Mesmas configurações, mas **Environment name:** `testpypi`

## Publicação

Uma vez configurado, a publicação acontece automaticamente:
- **No release:** Crie um release no GitHub → o pacote é publicado automaticamente no PyPI
- **Manual:** Vá em Actions → "Publish to PyPI" → Run workflow

## Instalação

```bash
pip install sentinel-guardrails
```

O comando CLI `sentinel` e todos os imports `from sentinel import ...` funcionam da mesma forma.
