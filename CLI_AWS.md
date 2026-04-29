**AWS CLI**
___
O AWS CLI é a interface de linha de comando oficial da AWS. Essa funcionalidade **permite controlar praticamente todos os serviços via terminal**, sem tocar no console web.   
Com uma autenticação pré-definida da access key + secret key o CLI faz requisições de API na AWS que responde e executa a ação.   

Exemplos mais comuns de uso:
```bash
# Configuração inicial da AWS CLI (define credenciais e região)
aws configure

# Verifica qual usuário/conta está autenticado (evita operar na conta errada)
aws sts get-caller-identity


# =========================
# S3 (armazenamento)
# =========================

# Lista todos os buckets
aws s3 ls

# Cria um novo bucket
aws s3 mb s3://meu-bucket

# Envia um arquivo para o bucket
aws s3 cp arquivo.txt s3://meu-bucket/

# Sincroniza um diretório local com o bucket (muito usado em deploy)
aws s3 sync . s3://meu-bucket/


# =========================
# EC2 (máquinas virtuais)
# =========================

# Lista todas as instâncias EC2
aws ec2 describe-instances

# Cria uma nova instância (VM)
aws ec2 run-instances \
  --image-id ami-xxx \
  --instance-type t2.micro \
  --count 1

# Para uma instância
aws ec2 stop-instances --instance-ids i-123

# Termina (deleta) uma instância
aws ec2 terminate-instances --instance-ids i-123


# =========================
# IAM (acessos e usuários)
# =========================

# Lista usuários IAM
aws iam list-users

# Cria um novo usuário IAM
aws iam create-user --user-name nome


# =========================
# Ajuda
# =========================

# Mostra ajuda geral da AWS CLI
aws help
```
___
> ***Quando usar CLI e quando usar CDK?***
> 
> **CLI: comandos rápidos, testes e debbuging.**   
> **CDK: ambiente reproduzível, tem mais de 1 recurso, é fácil de documentar (é um arquivo), controle real de infra.**
