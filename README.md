# Terraform CI/CD centralizado
Github Actions para ser reutilizado nos projetos de Terraform, a pipeline faz a validação da sintaxe do código terraform, gera alterações previstas utilizando o terraform plan, efetua validações de segurança, efetua cálculo estimado do custo de infraestrutura e cria/altera recursos em sua conta da AWS. Lista de comandos que serão executados abaixo:

- terraform init OR terraform init with backend
- terraform fmt
- terraform validate
- validação tfsec
- terraform plan
- generate_infracost
- post_infracost
- terraform apply

## Inputs
| Nome | Descrição | Requirida |Default |
|------|-----------|-----------|--------|
|`tf_version` | Versão do terraform | não | 1.0.0 |
|`os_version` | Versão do sistema operacional | não | ubuntu-20.04 |
|`use_state_bucket`| Flag utilizada para habilitar o uso de bucket para persistência de estado | não | false |
|`bucket_name`| Utilizado para informar o nome do bucket para persistência de estado | não | null |
|`key_tf_state`| Utilizado para informar o path do bucket para persistência de estado | não | null |
|`dynamodb_table`| Utilizado para informar a tabela do bucket para persistência de estado | não | null |
|`use_infracost`| Flag utilizada para habilitar o uso do InfraCost para cálculo estimado do custo de Infraestrutura | não | false |

## Como utilizar 
Criar a seguintes estrutura de diretórios: 

`.github/workflows/<proposito>.yml`

Utilize o exemplo abaixo para seu pipeline de CI:

```yaml
name: "Pipe Terraform"
on:
  push:
    branches:
      - '*'
  pull_request:
jobs:
  terraform:
      permissions: write-all
      uses: "yagoalmeida/terraform_actions/.github/workflows/terraform_actions.yaml@main"
      with:
        tf_version: "1.0.9"
        os_version: "ubuntu-20.04"
        use_state_bucket: true
        bucket_name: "shared-storage-terraform-state-dev"
        use_infracost: true
      secrets:
        aws_access_key_id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        infracost_api_key: ${{ secrets.INFRACOST_API_KEY }}
```
