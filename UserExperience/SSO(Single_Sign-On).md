**SSO (Single Sign-On)**

---

Permite que o usuário **faça login uma única vez e acesse múltiplos sistemas sem precisar autenticar novamente**.
Muito usado em hubs de aplicações, sistemas internos e integrações com provedores externos.

Dessa forma você elimina múltiplos logins e centraliza a autenticação.

> *Como funciona?*
> *R: Seu sistema redireciona o usuário para um provedor de identidade, que autentica e retorna um token validado.*

Os protocolos mais comuns são OAuth 2.0 e OpenID Connect.

Exemplo 1 de fluxo:

```text
1. Usuário acessa seu sistema
2. Sistema redireciona para o provedor (ex: Google)
3. Usuário faz login
4. Provedor retorna um token
5. Sistema valida o token
6. Usuário entra autenticado
```

---

> `• login único`
> `• sem múltiplas senhas`
> `• acesso centralizado`

Exemplo 2 de uso (conceito em código - validação de token):

```typescript
import jwt from 'jsonwebtoken';

function validateToken(token: string) {
  try {
    const decoded = jwt.verify(token, 'PUBLIC_KEY');
    return decoded;
  } catch (err) {
    throw new Error('Invalid token');
  }
}
```

---

> `• recebe token do provedor`
> `• valida assinatura`
> `• libera acesso ao sistema`

Ferramentas comuns:

* Auth0
* Firebase
* AWS Cognito

---

> `1 login → múltiplos sistemas`
> `menos fricção`
> `melhor controle de acesso`

SSO não é sobre “tipo de usuário” mas sim sobre **centralizar autenticação** e escalar acesso de forma consistente.
