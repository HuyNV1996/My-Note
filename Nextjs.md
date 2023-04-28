#### Pre-rendering là gì
Pre-rendering: Next.js sẽ tạo trước HTML cho từng trang, thay vì tất cả được thực hiện ở client như Reactjs.
#### Có mấy loại pre-rendering
* Static generation: HTML sẽ được generate tất cả ngay từ đầu và được sử dụng mỗi lần request.
* Server-side rendering: HTML sẽ được generate mỗi lần request.
> Pre-rendering giúp trang web của chúng ta có hiệu năng và khả năng SEO tốt hơn
#####  Static generation không có data
* Đối với trường hợp page được tạo mà không cần lấy data từ bên ngoài (API, file system, ...), các trang sẽ được generate ngay từ lúc buildtime
#####  Static generation có data
* Không phải lúc nào mà các page của chúng ta cũng chỉ là HTML mà không có data từ bên ngoài, có thể bạn sẽ cần lấy data từ API của bên thứ 3, hay lấy từ file system, ... và Next.js cũng hỗ trợ luôn.
* getStaticProps => Nó sẽ nói với Nextjs là page này có 1 số data lấy từ bên ngoài, khi pre-render cần lấy data để sử dụng.
* getStaticProps => Chỉ chạy ở server side
* getStaticProps sẽ chỉ chạy được ở trong pages mà không chạy được ở component thường
* Bạn nên sử dụng getStaticPaths nếu bạn đang pre-render các trang tĩnh và sử dụng dynamic routes và data được trả về từ headless CMS, database,
* getStaticPaths phải được sử dụng với getStaticProps.

// pages/posts/[id].js

```js
function Post({ post }) {
  // Render post...
}

 
// The following function is called at build time
export async function getStaticPaths() {
  // To obtain posts, use an external API endpoint.
  const res = await fetch('https://.../posts')
  const posts = await res.json()

 
  // Obtain the pathways we wish to render ahead of time based on postings.
  const paths = posts.map((post) => ({
    params: { id: post.id },
  }))

 
  // Only these pathways will be pre-rendered at build time.
  // Other routes should be 404 if fallback: false is set.
  return { paths, fallback: false }
}

 
// This also gets called at build time
export async function getStaticProps({ params }) {
  // params contains the post `id`.
  // params.id is 1 if the route is like /posts/1
  const res = await fetch(`https://.../posts/${params.id}`)
  const post = await res.json()

 
  // Props are used to pass post data to the page.
  return { props: { post } }
}

 
export default Post
```
#### Tham khảo
https://www.codingninjas.com/codestudio/library/static-page-generation-using-getstaticpaths-in-next-js

https://www.codingninjas.com/codestudio/library/data-fetching-using-getstaticprops

https://www.codingninjas.com/codestudio/library/incremental-static-regeneration
##### Static Site Generation
SSG là quá trình các file html,css,js sẽ được sinh ra tại thời điểm build time.
```js
export async function getStaticPaths () {
   const { data } = await axios.get(`https://restcountries.com/v3.1/all`);
   
//    const countries = data.map(country => country.name.common)  
   return {
    fallback: false,
    paths: data.map( country => ({ params: { Singlecountry: country.cca3.toString()}}) )
    } 
} 

export async function getStaticProps(context) {

    const country = context.params.Singlecountry;
    const {data} = await axios.get(
      `https://restcountries.com/v3.1/alpha/${country}`
    );
    
 return {
    props: {
        data
    }
 };

}
```
FAQs
#### What is data fetching in next.js?
The data fetching in Next.js allows the user to change the application's content. This includes pre-rendering the content with Server-side Rendering or Static Generation and updating or creating content at the runtime with Incremental Static Regeneration.
 
#### What are the ways of fetching data in nextJs?
The different strategies used for fetching data are; Client-side rendering, Server-side rendering, Static site generation, and ​Incremental static regeneration.
 
#### When should I use Static Site Generation?
The getstaticProps method should be used to fetch an application's data that doesn’t change often.


#### Incremental Static Regeneration
Có 1 vấn đề của Static Site Generation là data chỉ update tại thời điểm build time. Khi đó ta muốn update data mới cần phải build lại. Để giải quyết vấn đề này Incremental Static Regeneration nó ra đời giúp việc update data không cần build lại. Chúng ta sẽ có 1 tham số revalidate: 100, // In seconds trong fuction getStaticProps() giúp ta update sau cứ sau 1 thời gian hẹn trước mà không cần build lại.

Ở phương pháp trên vẫn có nhược điểm là data sẽ không thể update tức thời. Ví dụ setup 60s sẽ update data/ lần trong 60s đó người dùng sẽ không thể nhìn thấy sự thay đổi ngay lập tức. Vì vậy, từ phiên bản v12.2.0 Next Js hỗ trợ thêm On-Demand Incremental Static Regeneration giúp ta có thể chủ động update data bất cứ khi nào CMS update dữ liệu.