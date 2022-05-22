## 

## Intro

```
auth : corner
date : 05-22
```

#### Description✍️



> 💡Nuxt에서 Video를 배경으로 사용하는 방법은 몇 가지 있지만,
>
> ​	 플레이어에 비디오를 넣기 위해서는 파일을 직접 local이나 배포되는 서버에 올리지 않고 Cloudinary라는 방법으로 제시하고 있습니다.
>
> ​	 Nuxt에서 Video를 배경으로 사용하는 방법을 제공해주고 있습니다. 





 [![img](https://camo.githubusercontent.com/fbc3df79ffe1a99e482b154b29262ecbb10d6ee4ed22faa82683aa653d72c4e1/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4769744875622d3130303030303f7374796c653d666f722d7468652d6261646765266c6f676f3d676974687562266c6f676f436f6c6f723d7768697465)](https://github.com/eight-corner)  [Github 소스](https://github.com/Eight-Corner/corner-video-plugin)



---



## Setup



`1.` 프로젝트를 생성합니다.

```bash
npm init nuxt-app project_name
```



![](https://velog.velcdn.com/images/corner3499/post/14d77a57-1790-4f94-bf26-0217ae3bd7b3/image.png)





`nuxt.config.js` 에 head에다가 script와 stylesheet를 추가합니다.

```js
<!----------style-------------->
{ rel:  'stylesheet', href:  'https://unpkg.com/cloudinary-video-player@1.5.9/dist/cld-video-player.min.css' }
<!-- ------- script-------->
{ src:  'https://unpkg.com/cloudinary-core@latest/cloudinary-core-shrinkwrap.min.js'  },
{ src:  'https://unpkg.com/cloudinary-video-player@1.5.9/dist/cld-video-player.min.js'  }
```

## 

`2.` `.env` 파일을 생성합니다. 

```bash
touch .env
```

example

```
NUXT_ENV_CLOUDINARY_CLOUD_NAME=username
```

CLOUDINARY_CLOUD_NAME을 알려는 방법은 이곳을 참조하세요.

[대시보드](https://cloudinary.com/console/c-47db719634378093509b54c45fd745)에 들어가면 화면에 Account Detail에 Cloud Name을 알려주고 있습니다.



Cloudinary의 계정이 없다면 깃허브나 구글을 이용해서 가입을 하시길 권장합니다.



`3.` Cloudinary에서 Media Library 메뉴로 이동하여 Folder를 추가합니다.

Folder명은 임의로 지정하거나, 코드상에서 public id값과 동일해야 합니다.

Media Library에서 `nuxtjs-video-recommendations`라는 폴더명을 생성하고, video를 추가합니다. 



`4.` 추가한 비디오에 나오는 제목이 `public Id = "폴더명/비디오제목"`으로 연결되는 것을 알고있어야 합니다.



`5.` 프로젝트로 들어와서 비디오가 들어가야 할 페이지에서 작업합니다.

`Index.vue`

```vue
 <video
      id="recommendations-player"
      controls
      muted
      class="cld-video-player cld-video-player-skin-dark w-2/3 h-96 mx-auto"
    >
</video>
```

```js
export default {
  name: 'IndexPage',
  data(){
    return {
      cld:null,
      player:null,
      source1: {
        publicId: "nuxtjs-video-recommendations/fireplace_bg_hoxtgx",
        title:'Night Street',
        subtitle:'Street at night with traffic and pedestrians',
        description:'Street at night with traffic and pedestrians'
      },
      source2: {
        publicId: "nuxtjs-video-recommendations/nightsky_bg_qt5bpq\n",
        title:'Cookie',
        subtitle:'Decorating a Cupcake with Gingerbread Cookie',
        description:'Decorating a Cupcake with Gingerbread Cookie'
      },

    };
  },
  mounted(){
    this.source1.recommendations = [
      this.source2
    ];

    this.cld = cloudinary.Cloudinary.new({
      cloud_name: process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME,
      secure: true,
      transformation: {crop: 'limit', width: 300, height:900}
    });

    this.player = this.cld.videoPlayer('recommendations-player',
      {
        autoShowRecommendations: true,
        sourceTypes: ['mp4']
      }
    );

    this.player.source(this.source1);
  }
}
```



`source1, source2`의 publicId 키 값에 `3.`번과  `4.`번에서 말한 값이 들어가야합니다. 



| **Attributes** |                                              |        |
| -------------- | -------------------------------------------- | ------ |
| 속성           | 기능                                         | string |
| id             | this.cld.videoPlayer()에 들어갈 id 값입니다. | bool   |
| controls       | 플레이어의 컨트롤 기능 on/off 여부           | bool   |
| muted          | 음소거 기능                                  | bool   |
| loop           | 무한 루프                                    | bool   |
| ...            |                                              |        |

그외 추가 적인 속성은 추후 작성하겠습니다.
