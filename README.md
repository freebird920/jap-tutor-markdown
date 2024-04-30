# jap-tutor-Readme

1. [CHAT](/chat)
1. [BLOG](/blog)

### 목차
1. [목차](#목차)
1. [jap-tutor 소개](#jap-tutor-소개)
1. [jap-tutor-build](#japtutor-build)
    1. [auth](#auth)
1. [chat services](#chat-services)

## jap-tutor 소개
jap tutor은 일본어를 쉽게 학습할 수 있도록 도와주는 사이트입니다. 

## oauth
소셜 로그인(oauth)는 auth.js를 사용해서 구현하였습니다. 소셜 로그인에서 개인정보는 일절 수집하지 않습니다. 

## japtutor build

### auth
지금까지는 vercel을 이용하여 배포하였기 때문에 nextauth.js v4와 mongodb를 이용하였습니다. 그런데 cloudflare pages를 이용하여 배포하려고 하니 문제가 생겼습니다. cloudflare pages는 node.js 환경이 아니라 edge runtime으로 구동 되기 때문입니다. 

edge runtime에서 oauth를 구현하기 위해 auth.js v5를 사용하였습니다. auth.js v5는 아직 beta라고 하고 문서도 아직 잘 갖추어 지지 않아서 주저했는데 의외로 편하게 구성할 수 있었습니다. 무엇보다도 next.js의 app router에서는 제대로 작동하지 않았던 custom page를 구성할 수 있게 되었습니다. 

slat.or.kr에 auth.js v5를 적용해보았는데 심각한 문제가 있습니다. auth.js v5를 적용하면 naver oauth를 이용할 수 없습니다. naver oauth callback 타입 문제인 것 같은데 아직 어떻게 해결해야할지 모르겠습니다. 

```typescript
import { handlers } from "@/auth"; // Referring to the auth.ts we just created
export const { GET, POST } = handlers;
export const runtime = "edge" 
```

```typescript
import NextAuth from "next-auth";
import Kakao from "next-auth/providers/kakao";
import GitHub from "next-auth/providers/github";
export const { handlers, signIn, signOut, auth } = NextAuth({
  providers: [GitHub, Kakao],
  callbacks: {
    jwt({ token, user, account }) {
      if (user) {
        // User is available during sign-in
        token.id = user.id;
      }
      if (account) {
        token.providerAccountId = `${account.provider}${account.providerAccountId}`;
      }
      return token;
    },
    session({ session, token }) {
      session.user.providerAccountId = token.providerAccountId;
      return session;
    },
  },
});

```

## chat services

반갑습니다. JAP-TUTOR에 오신 것을 환영합니다.
반갑습니다. JAP-TUTOR에 오신 것을 환영합니다.
반갑습니다. JAP-TUTOR에 오신 것을 환영합니다.
Welcome to JAP-TUTOR. GO!
Welcome to JAP-TUTOR. GO!
Welcome to JAP-TUTOR. GO!
ようこそJAP-TUTORへ。日本語の勉強を始めましょう。
ようこそJAP-TUTORへ。日本語の勉強を始めましょう。
ようこそJAP-TUTORへ。日本語の勉強を始めましょう。

