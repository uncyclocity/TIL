# 4. Next.js의 데이터 패치 메소드

> References <br> <a href="https://develogger.kro.kr/blog/LKHcoding/133">"getInitialProps와 getServerSideProps 비교 정리"</a>_LKHcoding_ <br> <a href="https://velog.io/@devstone/Next.js-100-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0-feat.-initialProps-webpack-storybook">"Next.js 100% 활용하기 (feat. getInitialProps, getStaticPath, getStaticProps, getServerSideProps, storybook)"</a> _devstone_ <br> <a href="https://chaeyoung2.tistory.com/53">"next.js 데이터 가져오기 getServerSideprops, getStaticProps, getStaticPaths"</a>_2챙_

## 1. getInitialProps

- **SSR**을 구현할 때 사용된다.
- 빌드와 관계 없이 페이지 요청이 이루어 질 때마다 데이터를 가져온다.
- 데이터 패치만 **서버에서 담당**하고, 렌더링에 필요한 연산은 **클라이언트에서 담당**한다.
- 빌드가 된 뒤에는 컴포넌트가 로딩 되기 전에 실행된다.

## 2. getServerSideProps

- **SSR**을 구현할 때 사용된다.
- 빌드와 관계 없이 페이지 요청이 이루어 질 때마다 데이터를 받아온다.
- 데이터 패치와 렌더링 연산을 **모두 서버에서 담당**한다.  

## 3. getStaticProps

- **SSG(Static Site Generations)**를 구현할 때 사용된다.
- 일반적으로 빌드 시 받은 데이터는 이후 **고정**된다.
- 새 데이터를 받는 주기를 설정할 수 있다. 

## 4. getStaticPaths

- getStaticProps를 사용하면서 **동적 라우팅**을 사용할 경우 필요하다.
- 만약 미리 빌드하도록 정의 한 페이지 이외의 다른 페이지 요청이 들어올 경우, **fallback** 옵션을 통해 빌드 로딩과 이후 띄워질 수 있도록 설정할 수 있다.

[id].js

```javascript
function Page({ data }) {
 const router = useRouter()

  if (router.isFallback) {
    return <div>Loading...</div>
  } else {
    return (
      <ul>
        {data.map((post) => (
          <li>{post.title}</li>
        ))}
      </ul>
    )
  }
}

export async function getStaticPaths() {
  const posts = await axios.get("https://.../posts");
  
  // params: {id : '1'},{id : '2'}...
  const paths = posts.map(({ id }) => ({ params: { id: `${id}` } }));
  
  // fallback : 만들어지지 않은 페이지도 로딩 후 만들어주겠다는 의미
  // false일 경우 404를 리턴한다.
  return {
    paths,
    fallback: true,
  };
}

export async function getStaticProps({ params }) {
  const res = await axios.get(`https://localhost:3000/posts/${params.id}`)
  const data = res.data

  return { props: { data } }
}
```
