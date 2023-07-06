# 환경 변수 (env)

[공식 문서 - env](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)

Next.js는 내장된 환경 변수 지원을 제공하여 다음과 같은 기능을 사용할 수 있습니다.

## 서버 컴포넌트 (Server Components)

서버 컴포넌트에서 환경 변수를 사용하려면 `.env.local` 파일을 생성하여 환경 변수를 로드해야 합니다.

```plaintext
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

위의 환경 변수는 `process.env.DB_HOST`, `process.env.DB_USER`, `process.env.DB_PASS`와 같이 접근하여 사용할 수 있습니다. 이러한 변수에는 서버 컴포넌트 또는 `getStaticProps`와 같은 메서드에서만 접근할 수 있으며, 브라우저 컴포넌트에서는 접근할 수 없습니다.

```javascript
export async function getStaticProps() {
  const db = await myDB.connect({
    host: process.env.DB_HOST,
    username: process.env.DB_USER,
    password: process.env.DB_PASS,
  })
  // ...
}
```

## 클라이언트 컴포넌트 (Client Components)

클라이언트 컴포넌트에서 환경 변수에 접근하기 위해 변수 이름 앞에 `NEXT_PUBLIC_` 접두사를 추가해야 합니다.

```plaintext
NEXT_PUBLIC_API_KEY=abcdefghijk
```

서버 컴포넌트에서는 `NEXT_PUBLIC_` 접두사를 사용하지 않는 것이 좋습니다. 이는 보안에 취약할 수 있기 때문입니다.

이제, 클라이언트 컴포넌트에서 이러한 환경 변수에 접근하려면 다음과 같은 코드를 사용할 수 있습니다.

```tsx
const ACCESS_TOKEN = process.env.NEXT_PUBLIC_API_KEY;

const axiosInstance = axios.create({
  baseURL: process.env.NEXT_PUBLIC_BASE_URL,
  headers: { Authorization: `Bearer ${ACCESS_TOKEN}` },
});
```
