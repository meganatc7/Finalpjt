# Final Project - Show your movie

![](./design/logo_black.png)

![Langueage](https://img.shields.io/badge/python-v3.8.7-important) ![JS](https://img.shields.io/badge/JS-ES6-important) ![node](https://img.shields.io/badge/node-v12.7.0-important) ![vue](https://img.shields.io/badge/vue-@vue/cli/4.4.1-important) ![API](https://img.shields.io/badge/API-2-lightgrey) ![language](https://img.shields.io/badge/language-korean-red) ![movie](https://img.shields.io/badge/movie-tmdb-green) ![video](https://img.shields.io/badge/video-youtube-green)



## ๐INDEX

#### Introduction

- What is SGmovies?
- ERD
- UI Design



#### Features

- Movie.json
- Sign up/Login
- Movie filter & Search
- Comment
- Trail
- Profile
- Recommend



#### Documents

- Docs
- Issues





## ๐ฌWhat is SGmovies?

> **์ํ ๋ชฉ๋ก / ๊ฒ์ / ์ํ ์ถ์ฒ / ํ์  / ๋ฆฌ๋ทฐ ๋๊ธ** ๊ธฐ๋ฅ์ ๋ํ ์ํ ๋ฐ์ดํฐ ์๋น์ค๋ฅผ ์ ๊ณตํด์ฃผ๋ ํ๋ก์ ํธ์๋๋ค.

- ์ฌ๋ฏธ์๊ฒ ๋ณธ ์ํ๋ฅผ ๊ณจ๋ผ์ ์๋ก์ด ์ํ๋ฅผ ์ถ์ฒ๋ฐ์ผ์ธ์.
- ์ํ์ ๋ํ ํ์ ๊ณผ ํ์คํ๋ ๋จ๊ฒจ๋ณด์ธ์.

![์ ์ฒด ๋์๋](./design/structure_guide.png)



## ๐ERD structure

*draw.io ์ฌ์ฉ*

![](./design/ERD.png)



## ๐จUI Design

*Figma ์ฌ์ฉ*

1. ํ์๊ฐ์/๋ก๊ทธ์ธ ํ์ด์ง

![](./design/UI_1.png)

2. ํ๋กํ ํ์ด์ง

![](./design/UI_3.png)

3. ๋ฉ์ธ ํ์ด์ง

![](./design/UI_2.png)

4. ์ํ ์์ธ ํ์ด์ง

![](./design/UI_4.png)

![](./design/UI_5.png)

![](./design/UI_6.png)

5. ์ฝ๋ฉํธ ์๋ ฅ์ฐฝ ํ์ด์ง

![](./design/UI_7.png)



## ๐งFeatures

### 1. Movie.json(Movie ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ)

์ํ ์ ๋ณด๋ [TMDb](https://www.themoviedb.org/?language=ko), [kaggle](https://www.kaggle.com/)์์ ๋ฐ์์์ผ๋ฉฐ, Movie ๋ชจ๋ธ class์ field๊ฐ ๋ค์ด๊ฐ ์ ์๋๋ก ์์ฑํ์์ต๋๋ค. 

data๋ฅผ ๊ฐ์ ธ์ค๊ธฐ ์ํด `requests`ํจํค์ง๋ฅผ ํตํด API Server๋ก ์์ฒญํ, python์ผ๋ก jsonํ์ผ์ ๋ง๋ค์์ผ๋ฉฐ, for๊ณผ if๋ฅผ ์ฌ์ฉํ์ฌ ๋ชจ๋ธ๋ง ๊ตฌ์กฐ์ ๋ง์ท์ต๋๋ค.

```python
for i in range(0,len(id_lst)):
    URL = f'{url}/{category}/{id_lst[i]}?api_key={key}&language=ko-KR'
    res = requests.get(URL)
    res = res.json()
    
    if res.get('overview') and res.get('genres'):
        tmp = {}
        tmp2 = {}
        tmp['model'] = 'movies.movie'
        tmp['pk'] = i + 1
        tmp2['title'] = res['title']
        tmp2['original_title'] = res['original_title']
        tmp2['overview'] = res['overview']
        tmp2['language'] = res['original_language']
        tmp2['poster'] = f'https://www.themoviedb.org/t/p/original/{res["poster_path"]}'
        tmp2['genres'] = ''
        genres_tmp = []
        for i in res['genres']:
            genres_tmp.append(i['name'])
        tmp2['genres'] = ' '.join(genres_tmp)
        tmp2['runtime'] = res['runtime']
        tmp2['release_date'] = res['release_date']
        tmp2['vote_average'] = res['vote_average']
        tmp2['vote_count'] = res['vote_count']
        tmp['fields'] = tmp2

        movie_lst.append(tmp)
```

์ด๋ ๊ฒ ์ถ๋ ฅํ `movie.json`์ ๋ฐฑ์๋์์ ์ฌ์ฉ๋  ๊ฒ์ด๋ฏ๋ก `movies.app/fixtures/movies`์์ ๋ฃ์ด์ฃผ๊ณ  django์์ `loaddata`ํด์ค๋๋ค.

```bash
โจDB ๋ฐ์
$ python manage.py loaddata data/moviedata.json
```



### 2. Sign up/Login

์ฌ์ฉ์์๊ฒ ์๋ ฅ ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ๋ฐฑ์๋ ์๋ฒ๋ก ์ ์กํ์ฌ ํ์๊ฐ์๊ณผ ๋ก๊ทธ์ธ ๊ณผ์ ์ ์๋ฃํฉ๋๋ค.

#### 01 - Sign up(Backend)

ERD์ ๋์ผํ๊ฒ ๋ชจ๋ธ๋ง์ ์์ฑํ์์ผ๋ฉด User serialize๋ฅผ ์งํํด์ค๋๋ค.  ํ์๊ฐ์ ์ ์ฐจ๊ฐ ์งํ๋  ์ ์๋๋ก url์ ๋ง๋ค์ด์ค๋๋ค.

```python
urlpatterns = [
    path('signup/', views.signup),
]
```

ํ์๊ฐ์์ ๋ก์ง์ views.py ์ ์์ฑํ์์ต๋๋ค. ๋ก์ง์ ์ฃผ์ ๋ด์ฉ์ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.

1. ๋น๋ฐ๋ฒํธ(password) / ๋น๋ฐ๋ฒํธ ํ์ธ(password confirmation)์ ์ฌ์ฉ์๊ฐ ์๋ ฅํ ๊ฐ ์ ์ฅ
2. ๋ง์ฝ ๋น๋ฐ๋ฒํธ์ ๋น๋ฐ๋ฒํธ ํ์ธ์ ๊ฐ์ด ๋ค๋ฅด๋ฉด 400 state ์ฝ๋๋ฅผ ๋ณด๋ด์ค๋๋ค.
3. ๊ทธ๋ ์ง ์๋ค๋ฉด ์ง๋ ฌํ ํด์ค๋ค, User ๊ฐ์ ์ ์ฅํด์ค๋๋ค.
4. ์ด๋, user๊ฐ ์ข์ํ๋ ์ํ ๋ชฉ๋ก๋ ํ์ด๋ธ์ ์์ฑํด์ค๋๋ค.

```python
@api_view(['POST'])
def signup(request):
    password = request.data.get('password')
    password_confirmation = request.data.get('password_confirmation')

    if password != password_confirmation:
        return Response(status=status.HTTP_400_BAD_REQUEST)

    serializer = UserSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        user = serializer.save()
        user.set_password(request.data.get('password'))
        user.save()
        Userlike(user=user).save() 
        return Response(data=serializer.data)
```

๋ชจ๋  return์ response๋ก ๋ณด๋ด์ค๋๋ค.



#### 02 - Sign up(Frontend)

ํฌํ๋ฆฟ์์ DB์ ์ ์ฅ๋์ด์ผ ํ๋ ๊ฐ๋ค์ธ Username, Email, Password, Password Confirmation์ ๊ฐ์ ๋ฐ์์ต๋๋ค. `v-if`๋ฅผ ์ฌ์ฉํ์ฌ ๋ง์ฝ ์ ์ ํ์๊ฐ์์ด ๋๋ฉด, ์๋ด๋ฌธ๊ตฌ๊ฐ ๋น๊ฒ ํด์ค๋๋ค.

user์ ๋ฐ์ดํฐ๋ store์ ์ ์ฅํด์ฃผ๋๋ก ํฉ๋๋ค. User ์ ๋ณด๋ฅผ ์ ์ฅํด์ฃผ๋ mutation์ ๋ง๋ค์ด input ๋ฐ์ดํฐ๋ฅผ ๋ฐ๊ฒ ๋๋ฉด ์ด๋ฅผ ์ฌ์ฉํ์ฌ Django server๋ก ๋ฐ์ดํฐ๋ฅผ ์ ์กํด์ค๋๋ค.

```javascript
// store์์ Userinfo ๋ฐ์ดํฐ๋ฅผ ๋ค๋ฃจ๊ธฐ ์ํ getters
getUserCreateStatus(state) {
    return state.userCreateStatus
},

// user ์ ๋ณด๋ฅผ ์ ์ฅํด์ฃผ๋ mutation
CREATE_USER(state, userCreateData) {
    state.userInfo = userCreateData.userInfo
    state.userCreateStatus = userCreateData.status
},
```

์ฌ์ฉ๋ ์ฝ๋๋ค์ ์์ ๊ฐ์ต๋๋ค.



#### 03  - Login(Backend)

๋ก๊ทธ์ธ ์ ๋ณด๋ฅผ ๋๊ธฐ๊ธฐ ์ํด์ JWT Token์ ์ฌ์ฉํ์์ต๋๋ค. ํ ํฐ์ user์ ๋ณด์ ํจ๊ป ์ธ์ฆ ์ ๋ณด๊ฐ ๋ด๊ฒจ์ server-client๋ฅผ ์ค๊ฐ๊ฒ ๋ง๋ค์์ต๋๋ค.

```python
urlpatterns = [
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
]
```

ํ ํฐ์ ๋ณด๋ด์ฃผ๋ login url์ ๋ง๋ค์ด์คฌ์ต๋๋ค. client์์ ์ด url์ ์ฌ์ฉํ์ฌ server์ ์์ฒญ์ ๋ณด๋ผ ๋, ์๋์ผ๋ก ํ ํฐ์ด ํจ๊ป ๋ณด๋ด์ง๊ฒ ๋ฉ๋๋ค.

```javascript
async AUTH_USER({ commit }, userInfo) {
    const AUTH_USER_URL = '/api/token/'
    const data = userInfo
    const response = await axios.post(AUTH_USER_URL, data)

    const token = response.data.access
    const payload = {'username': userInfo.username, 'token': token}
    localStorage.setItem('token', token)
    commit('AUTH_USER', payload)
},
```

ํ ํฐ์ ์ ๋ณด๋ฅผ ๋ณด๋ด์ฃผ๋ ๊ธฐ๋ฅ์ actions์ ์์ฑํด์ฃผ์์ต๋๋ค. ํ ํฐ์ local storage์ ์ ์ฅํด์ฃผ์์ผ๋ฉฐ, ์ฌ์ฉ์ ๊ณ์ ๋ถ๋ฌ์ฌ ๊ฒ์๋๋ค.



#### 04 - Login(Frontend)

์ฌ์ฉ์์๊ฒ input ๋ฐ์ดํฐ๋ก username, password๋ฅผ ๋ฐ์์ค๋๋ค. ๋ก๊ทธ์ธ ๋ฒํผ์ ๋๋ฅด๋ฉด ํด๋ฆญ ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ๊ฒ ๋๊ณ  ํ ํฐ๊ณผ ํจ๊ป ์๋ฒ์ ์์ฒญ์ ๋ณด๋ด๊ฒ ๋ฉ๋๋ค.

```javascript
methods: {
    onSubmit() {
        this.$store.dispatch('AUTH_USER', this.userInfo)
        this.$router.push('/')
    }
}
```

๋ก๊ทธ์ธ ๋ฒํผ ํด๋ฆญ ์ด๋ฒคํธ๋ `onSubmit`์ผ๋ก ์์ฑํ์์ต๋๋ค. ์ด๋ฒคํธ๊ฐ ์คํ๋๋ฉด actions์ `AUTH_USER`๊ฐ ์๋ํ์ฌ ๋ก๊ทธ์ธ์ด ์คํ๋ฉ๋๋ค.



### 3. Movie filter & Search

์ํ ์ฅ๋ฅด๋ณ ๋ถ๋ฅ์ ๊ฒ์ ๊ธฐ๋ฅ์๋๋ค.

#### 01 - Filter

![](./design/filter.png)

ํด๋น ์ฐฝ์์ ๊ฐ ์ฅ๋ฅด์ ๋งก๊ฒ ์๋ ์ถ๋ ฅ๋๋ ์ํ ๋ฆฌ์คํธ๊ฐ ํํฐ๋ง์ด ๋๋๋ก ํด์ฃผ๋ ์์์๋๋ค.

์ฌ๋ฌ ๋ฐฉ๋ฒ๋ค์ด ์๊ฒ ์ง๋ง, Django์๋ฒ์ ์์ฒญ์ ๋ณด๋ด์ ํด๋น ์ฅ๋ฅด์ ํด๋น๋๋ ์ํ ๋ฆฌ์คํธ๋ฅผ ๊ฐ์ ธ์ ์ถ๋ ฅํ๋ ๋ฐฉ๋ฒ์ ์ฌ์ฉํ์ต๋๋ค.

```html
<div class="me-5 d-flex align-items-center">
    <span class="fs-3 fw-bold content-font">๋ชจ๋  ์ํ</span>
    <div class="filter ms-5">
        <span class="content-font" :class="{ pick: state == 1 }" @click=all>์ ์ฒด</span>
        <span class="content-font" :class="{ pick: state == 2 }" @click=action>์ก์</span>
        <span class="content-font" :class="{ pick: state == 3 }" @click=comedy>์ฝ๋ฏธ๋</span>
        <span class="content-font" :class="{ pick: state == 4 }" @click=horror>๊ณตํฌ</span>
        <span class="content-font" :class="{ pick: state == 5 }" @click=animation>์ ๋๋ฉ์ด์</span>
        <span class="content-font" :class="{ pick: state == 6 }" @click=sf>SF</span>
    </div>
</div>
```

์ด๋ฐ ํํ๋ก ํํฐ๋ง ํ๋ UI๋ฅผ ๋ง๋ค์๊ณ  ๊ฐ๊ฐ์ ํด๋ฆญํ์ ๋ ํจ์๊ฐ ์คํ๋๋๋ก ํด์ฃผ์์ต๋๋ค.



๊ฐ๊ฐ์ ์ด๋ฒคํธ๋ ๋ค์๊ณผ ๊ฐ์ต๋๋ค.

```js
methods: 
	all() {
      this.state = 1
      this.$store.dispatch('FILTER_MOVIE','์ ์ฒด')
    },
    action() {
      this.state = 2
      this.$store.dispatch('FILTER_MOVIE','์ก์')
    },
    comedy() {
      this.state = 3
      this.$store.dispatch('FILTER_MOVIE','์ฝ๋ฏธ๋')
    },
}
```

๊ฐ๊ฐ์ ํ๊ทธ๋ฅผ ํด๋ฆญํ์ ๋, ๊ทธ ํ๊ทธ์ ํด๋นํ๋ ์นดํ๊ณ ๋ฆฌ ์ด๋ฆ์ ํจ๊ป `FILTER_MOVIE`๋ผ๋ acions์ ๋ณด๋ด์ฃผ์์ต๋๋ค.

๊ทธ๋ฌ๋ฉด actions์์ ์๋์ ๊ฐ์ด ๋ฐ์์ ์ฒ๋ฆฌํด์ฃผ๋๋ฐ,

```js
async FILTER_MOVIE({ commit }, category) {
    const FILTER_MOVIE_URL = `/api/v1/movies/${category}`
    const response = await axios.get(FILTER_MOVIE_URL)

    commit('FILTER_MOVIE', response.data)
    },
```

Django์๋ฒ์ ๋ฏธ๋ฆฌ ๋ง๋ค์ด ๋ URL๋ก ์นดํ๊ณ ๋ฆฌ ์ ๋ณด์ ํจ๊ป ์์ฒญ์ ๋ณด๋ด๊ณ  ์๋ต์ ๋ฐ์์ต๋๋ค. ๊ทธ๋ฆฌ๊ณ  ๋ฐ์์จ ์๋ต์ ์ด์  mutations์ผ๋ก commit์ ํตํด ๋๊ฒจ์ค๋๋ค.

acions์์ ๋ณด๋ด์ค ์์ฒญ์ ๋ฐ๋ผ Django์์๋ ํํฐ๋ง์ ์งํํ ๋ค ๊ฑธ๋ฌ์ง ์ํ ๋ฆฌ์คํธ๋ฅผ ๋ณด๋ด์ฃผ๋๋ฐ,

```django
@api_view(['GET'])
def filter_movie(request, category):
    if category == '์ ์ฒด':
        movies = Movie.objects.all()
    else:
        movies = Movie.objects.filter(genres__contains=category)
    

    serializer = MovieSerializer(movies, many=True)

    return Response(data=serializer.data)
```

์นดํ๊ณ ๋ฆฌ๊ฐ ์ ์ฒด์ผ ๊ฒฝ์ฐ์๋ ๋ชจ๋  ์ํ๋ฅผ, ์๋ ๊ฒฝ์ฐ ํด๋น ์นดํ๊ณ ๋ฆฌ๋ฅผ ํฌํจํ๊ณ  ์๋ ์ํ๋ค์ DB์์ ๋ฝ์ ๋๊ฒจ์ฃผ์์ต๋๋ค.

๋ง์ง๋ง์ผ๋ก ๋ค์ vue๋ก ๋์์ mutations์์

```vue
FILTER_MOVIE(state, filterMovieList) {
      state.movieList = filterMovieList
    },
```

actions์์ ๋ณด๋ธ ํํฐ๋ง ๋ ์ํ ๋ฆฌ์คํธ๋ฅผ ๋ฐ์์ state์ movieList์ ์ ์ฅํด์ฃผ๋ฉด ํํฐ๋ง์ด ๋์ด ๋ฉ๋๋ค.

์ฆ,` actions์์ ์์ฒญ > Django์๋ฒ์์ ์๋ต(ํํฐ๋ง๋ ์ํ๋ฆฌ์คํธ) > mutations์์ state ๋ณ๊ฒฝ`

์ด๋ฐ ํ๋ฆ์ ์๊ฐํ๊ณ  ์ฝ๋๋ฅผ ์์ฑํ์ต๋๋ค.



> **๋๋์ **
>
> ์๋ฌด๋๋ DB์ ์ ๊ทผ์ ํ๋ค๋ณด๋ ์๋๊ฐ ๋ง์ด ๋๋ฆฌ๋ค๋ ๋๋์ด ์์์ต๋๋ค. ๋์ค์ ์๊ฐ์ด ๋ ๊ฒ์ธ๋ฐ Vue์์ ์ฒ์์ ๊ฐ์ ธ์จ ์ํ ๋ฆฌ์คํธ๋ฅผ `forEach`๋ฅผ ํตํด์๋ ์ถฉ๋ถํ ๊ฐ๋ฅํ  ๊ฒ ๊ฐ๋ค๋ ์๊ฐ์ด ๋ค์์ต๋๋ค.
>
> ๋ค์์๋ ์กฐ๊ธ ๋ ๊ณ ๋ คํด์ DB์๋ ์ต์ํ์ผ๋ก ์ ๊ทผํ๋๋ก ํ๋ ๋ฐฉ๋ฒ์ผ๋ก ๊ตฌํ์ ํด๋ด์ผ๊ฒ ์ต๋๋ค.



#### 02 - Search

๊ฒ์๋ ๋ง์ฐฌ๊ฐ์ง๋ก ๊ฒ์์ฐฝ์ ์๋ ๊ฐ์ keyword๋ก ๊ฐ์ ธ์ Django ์๋ฒ์์ DB๋ฅผ ํ์ํด์ ๊ฐ์ ธ์ค๋ ํ๋ฆ์๋๋ค.

```html
<div class="" style="right:0;">
    <input 
       type="text" 
       class="me-3 search-box" 
       :class="{ searching: searching }"
       @input=onSearchInput
       v-model.trim=keyword
       placeholder="๊ฒ์"
     >
    <i class="fas fa-search fa-lg search" style="color:white;" @click=onSearch></i>
</div>

<script>
    ...
    data() {
       return {
           ...
           keyword: '',
       } 
    }
    methods: {
        ...
        onSearchInput() {
      		_.throttle(this.$store.dispatch('SEARCH_MOVIE', this.keyword),150)
    	}
    }
</script>
```

๊ฒ์์ฐฝ ๊ตฌ์กฐ๋ ์ด๋ฐ์์ผ๋ก v-model์ ํตํด์ ์๋ฐฉํฅ ๋ฐ์ธ๋ฉ์ ํด์ฃผ์๋ค.

๊ณต๋ฐฑ์ธ ๊ฒฝ์ฐ์๋ ๊ฒ์์ด ์๋๋๋ก trim์ ์ฌ์ฉํ์ต๋๋ค.



๊ฒ์์ฐฝ์ ์๋ ฅํ์ ๋์ @input ์ด๋ฒคํธ๊ฐ ๊ณ์ ์คํ๋๊ณ  ํ์ดํํ๋ ๋์ ๊ณ์ `SEARCH_MOVIE`๋ผ๋ actions๋ฅผ ์คํํฉ๋๋ค. lodash์ ์ค๋กํ๋ง์ ์ฌ์ฉํด์ ์์ฒญ์ด ์กฐ๊ธ์ ๋ ๊ฐ๊ฒ ๋ง์์ฃผ์์ต๋๋ค.

```js
async SEARCH_MOVIE({ commit }, keyword) {
    const SEARCH_MOVIE_URL = `/api/v1/movies/search/${keyword}`
    const response = await axios.get(SEARCH_MOVIE_URL)
    commit('SEARCH_MOVIE', response.data)
},
```

actions์์๋ ๋น๋๊ธฐ ์์ฒญ์ผ๋ก ๋ฏธ๋ฆฌ Django ์๋ฒ์ ๋ง๋ค์ด๋ URL๋ก ํค์๋์ ํจ๊ป ์์ฒญ์ด ๊ฐ๊ณ  ์๋ต์ด ์ค๋ฉด response ๋ณ์๋ก ๋ฐ์ ๋ค mutations๋ก ๋๊น๋๋ค.

```django
@api_view(['GET'])
def search_movie(request, keyword):
    reg = re.compile(r'[a-zA-Z]')

    if reg.match(keyword):
        movies = Movie.objects.filter(original_title__contains=keyword)
    else:
        movies = Movie.objects.filter(title__contains=keyword)
    
    serializer = MovieSerializer(movies, many=True)
    
    return Response(data=serializer.data)
```

Django์๋ฒ์์๋ ํค์๋์ ํจ๊ป ์์ฒญ์ ๋ฐ๊ณ  contains๋ฅผ ํตํด ํด๋น ํค์๋๋ฅผ ํฌํจํ๋ ์ํ ์ ๋ชฉ์ ๊ฐ์ง ์ํ๋ค์ ์น ๋ชจ์์ ๋ณด๋ด์ค๋๋ค.

```vue
SEARCH_MOVIE(state, searchedMovieList) {
      state.searchedMovieList = searchedMovieList
    },
```

๊ทธ๋ฆฌ๊ณ  mutations์์, django์์ ๋ฐ์ ์๋ต์ actions๋ฅผ ํตํด ๋ฐ์์ state์ ๊ฒ์๋ ์ํ ๋ฆฌ์คํธ๋ฅผ ์ ์ฅ์์ผ์ฃผ์์ต๋๋ค.

```html
    <div class="row row-cols-4" v-else>
      <MovieListItem
        v-for="movie in searchMovieList"
        :key="movie.pk"
        :movie="movie"
      />
    </div>
```

๋ฐ์์จ ๊ฒ์๋ ์ํ ๋ฆฌ์คํธ๋ v-for๋ฌธ์ ํ์ฉํด์ ํ๋ฉด์ ์ถ๋ ฅ์์ผ์ฃผ์์ต๋๋ค.



> **๋๋์ **
>
> ์ํ๋ ๋๋ก ์ ๊ตฌํ์ด ๋ ๊ฒ ๊ฐ์ต๋๋ค. ๋ฌผ๋ก  ์ฝ๋๊ฐ ์ ์ง์ฌ์ ธ์์ง ์์ง๋ง...
>
> ๋ถ์กฑํ์ง๋ง ๋๋ฆ ๋ง์กฑํ  ๋งํ ๊ฒฐ๊ณผ๊ฐ ๋์์ ๊ธฐ๋ถ์ด ์ข์์ต๋๋ค.
>
> ๋ค๋ง, ์กฐ๊ธ ์์ฌ์ ๋ ์ ์ ๊ฒ์์ ํ์ ๋ ๊ฒ์๋ ์ํ ์ ๋ชฉ์ ํด๋น ํค์๋๋ฅผ ํ์ด๋ผ์ดํ๊น์ง ํด์ฃผ๋ ๊ฒ๋ ํด๋ณด๊ณ  ์ถ์์ง๋ง ์๊ฐ์ด ์กฐ๊ธ์ ๋ถ์กฑํ๋ ๊ฒ ๊ฐ์ต๋๋ค.



### 4. Comment

์ฝ๋ฉํธ๋ฅผ ์์ฑํ๋ ๊ณต๊ฐ์ ์ํ ๋ํ์ผ ๋ชจ๋ฌ์ ์์ต๋๋ค.

![comment modal](./design/comment1.png)

์ฌ๊ธฐ์ ์ฝ๋ฉํธ ์์ฑ์ ์ํ ์์ด์ฝ์ ๋๋ฅด๋ฉด ModalView์์ Comment ์ปดํฌ๋ํธ๋ก ๋์ด๊ฐ๊ฒ ๋ฉ๋๋ค. ์ฆ, ์์ ์ด๋ฏธ์ง๋ Comment ์ปดํฌ๋ํธ ํ๋ฉด์๋๋ค.

![comment modal2](./design/comment2.png)

๋๊ธ์ ์์ฑํ๋ฉด ์์ ๊ฐ์ด ์์ฑ๋ ์ฝ๋ฉํธ๊ฐ username๊ณผ ํจ๊ป ๋ํ๋ฉ๋๋ค.



#### 01 - Backend

์ฝ๋ฉํธ ๋ชจ๋ธ class๋ ์์ [ERD](./design/ERD.png)์ ๊ฐ์ต๋๋ค. ๋ํ comment ์ปดํฌ๋ํธ์์ ๋ณด์ฌ์ง๋ ๋์์ ๋๊ฐ์ง ์๋๋ค.

1. ํด๋น ์ํ์ ์์ฑ๋ ์ฝ๋ฉํธ ๋ณด์ฌ์ฃผ๊ธฐ
2. ์ฝ๋ฉํธ ์์ฑํ๊ธฐ

์ฝ๋ฉํธ๋ฅผ ์์ฑํ๋ ๊ฒฝ์ฐ ํ์ ์ปดํฌ๋ํธ์ธ Comment Modal๋ก ๋์ด๊ฐ๋๋ค. 

๊ทธ๋ฆฌ๊ณ  ํ๋กํ์์ ์ฝ๋ฉํธ๋ฅผ ์์  ๋ฐ ์ญ์ ํ๋ ๊ธฐ๋ฅ์ด ์๊ธฐ ๋๋ฌธ์ ์ฝ๋ฉํธ ์์  ๊ธฐ๋ฅ ๋์๋ ์๊ตฌ๋ฉ๋๋ค.

๋ฐ๋ผ์ ์์ ๋ค ๋์์ ์ํด ํ์ํ ํจ์๋ค์ views.py์์ ์์ฑํฉ๋๋ค.



##### 1) ์ฝ๋ฉํธ ๋ณด์ฌ์ฃผ๊ธฐ

์ฝ๋ฉํธ๋ฅผ ๋ณด์ฌ์ฃผ๊ธฐ ์ํด์๋ DB์ ์ ์ฅ๋ ๋ชจ๋  ์ฝ๋ฉํธ ์ ๋ณด๋ฅผ ๊ฐ์ ธ์์ผํฉ๋๋ค. ํด๋น ์ํ์ ์์ฑ๋ ์ฝ๋ฉํธ๋ค์ ๋ณด์ฌ์ค ๊ฒ์ด๋ฏ๋ก movie id ์ ๋ณด๋ฅผ ๋ฐ์์ต๋๋ค.

์ํ ๋ด์ ์ฝ๋ฉํธ ์ ๋ณด๋ค์ ๋ชจ๋ ๊ฐ์ ธ์จ ๋ค, ํ ํ์ด์ง์ 4๊ฐ์ฉ ๋ณด์ด๋๋ก ์์ฑํด์ค๋๋ค. 

```python
@api_view(['GET'])
def comment_list(request, movie_id):
    comments = Comment.objects.filter(movie_id=movie_id).order_by('-pk')
    
    paginator = Paginator(comments, 4)

    page = request.GET.get('page')
    comments = paginator.get_page(page)

    serializer = CommentSerializer(comments, many=True)
    return Response(data=serializer.data)
```



##### 2) ์ฝ๋ฉํธ ์์ฑํ๊ธฐ

์ฝ๋ฉํธ ์์ฑ์๋ ์ํ ์ ๋ณด, user ์ ๋ณด, ์ฝ๋ฉํธ ๋ด์ฉ, ํ์ ์ด ํ์ํฉ๋๋ค.  ๋ฐ๋ผ์ ํ์ํ ์ ๋ณด๋ฅผ ์ฌ์ฉ์์๊ฒ์ ๋ฐ์ ์ ์ฅํด์ค๋๋ค. serializer์์ user์ movie ์ ๋ณด๋ Read Only๋ก ์ค์ ํ์๊ธฐ ๋๋ฌธ์  view.py ๋ด์์ ๋ ๋ค์ ํด์ค ํ์๊ฐ ์์ต๋๋ค. ๋ฐ๋ผ์ ๋ฐ์ดํฐ ๊ฐ์ฒด๋ฅผ ๋ง๋ค์ด DB์ ๋ด์ content, rank, username์ ์ ์ฅํด์ค ๋ค, serializeํ์ฌ DB์ ์ ์ฅํด์ค๋๋ค.

```python
@api_view(['POST'])
def create_comment(request):
    movie_id = request.data.get('movieId')
    username = request.data.get('userId')
    content = request.data.get('comment')
    rank = float(request.data.get('rank'))

    movie = get_object_or_404(Movie, id=movie_id)
    user = get_object_or_404(get_user_model(), username=username)
    data = {
        'content': content,
        'rank': rank,
        'username': username,
    }
    
    serializer = CommentSerializer(data=data)
    if serializer.is_valid(raise_exception=True):
        # ๋๊ธ ์ ๋ณด ์ํ์ ์ ์ฅ
        comment = serializer.save(movie=movie, user=user)
        return Response(data=serializer.data)
```



##### 3) ์ฝ๋ฉํธ ์์  ๋ฐ ์ญ์ 

์์ ๊ณผ ์ญ์ ๋ ์์ฒญ ๋ฐฉ์์ ๋ฐ๋ผ ๋์์ด ๋ฐ๋๊ฒ ๋ฉ๋๋ค. ๋ง์ฝ PUT method๋ก ๋ค์ด์ค๋ฉด ์์ , DELETE method๋ก ๋ค์ด์ค๋ฉด ์ญ์ ๋ฅผ ํ๊ฒ ๋ฉ๋๋ค.

comment๋ฅผ ํ๋ณํ๊ธฐ ์ํด pk๋ฅผ ์์ฒญ๊ณผ ํจ๊ป ๋ฐ์์์ค๋๋ค. ์ด์  ์์ฒญ์ ๋ฐ๋ผ ๋์์ ๋๋๋๋ฐ delete์ผ ๊ฒฝ์ฐ ๋ฐ์ดํฐ๋ฅผ ์ญ์ ํด์ฃผ๊ณ  ์ญ์ ๋์๋ค๋ ๋ฉ์ธ์ง๋ฅผ ๋ณด๋ด์ค๋๋ค.

put์ ๊ฒฝ์ฐ, ์ง๋ ฌํ๋ฅผ ์์ผ์ค ์์  ๋ ๋ด์ฉ์ด ๋ด๊ธฐ๊ฒ ๋ฉ๋๋ค. ๋ฐ๋ผ์ input์ ๋ด์ฉ์ ๋ค์ serialize๋ฅผ ํด์ค ๋ค, ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ํต๊ณผํ๋ฉด ์ ์ฅ์ด ๋๋๋ก ํฉ๋๋ค.



#### 02 - Frontend

์ฝ๋ฉํธ ์์ฑ / ์์  / ์ญ์  / ์ฝ๊ธฐ ๋ฅผ ์ํ ๋ชจ๋  ๋์์ ์ํด mutation๊ณผ action์ ์ฌ์ฉํ์์ต๋๋ค. Comment Modal ํ์ด์ง์์ ์ฝ๋ฉํธ๋ฅผ ์๋ก ์์ฑํ๊ฒ ๋๋ฉด, `CREATE_COMMENT`, ์ฝ๊ธฐ ์ํด ๊ฐ์ ธ์ค๋ ค๋ฉด `GET_MOVIE_COMMENT`์ ์ฌ์ฉํฉ๋๋ค.

```javascript
// โจ ์ฝ๋ฉํธ ์์ฑ

async CREATE_COMMENT({ commit }, comments){
    const COMMENT_CREATE_URL = '/api/v1/movies/createcomments/'
    const data = comments
    const response = await axios.post(COMMENT_CREATE_URL, data)
    const commentCreateData = {
        'comments': response.data,
    }
    commit('CREATE_COMMENT', commentCreateData)
},
    
// โจ์ฝ๋ฉํธ ๊ฐ์ ธ์ค๊ธฐ
async GET_MOVIE_COMMENT({ commit }, payload) {
    const movie_id = payload.movie_id
    const page = payload.page
    const GET_COMMENT_URL = `/api/v1/movies/${movie_id}/comments/?page=${page}`
    const response = await axios.get(GET_COMMENT_URL)
    commit('GET_MOVIE_COMMENT', response.data)
},
```

๊ฐ๊ฐ์ url์ ๋ง๋ค์ด actions์ ํตํด ํธ์ถํด ์ค๋๋ค.



Comment ์ญ์ ๋ Profile์ปดํฌ๋ํธ์ ํ์๋ก ๋ค์ด์๋ Profile Movie List Item ์ปดํฌ๋ํธ์์ ํ  ์ ์์ต๋๋ค. ๋๊ธ ์ญ์ ์ ๊ฒฝ์ฐ comment๋ฅผ props๋ก ๋ฐ์์์ผํฉ๋๋ค. ์ญ์  ๋ฒํผ์ ๋๋ฅด๋ฉด ์ด๋ฒคํธ์ ํจ๊ป `DELETE_COMMENT` ์ก์์ ์คํํ๋๋ก ํฉ๋๋ค.

```javascript
async DELETE_COMMENT({ commit }, comment) {
    const COMMENT_DELETE_URL = `/api/v1/movies/comments/${comment.id}/`
    let response = await axios.delete(COMMENT_DELETE_URL)
    response = comment
    commit('DELETE_COMMENT', response)
},
```



Comment ์์ ์ Profile Movie List Item ์ปดํฌ๋ํธ์ ํ์๋ก ๋ค์ด์๋ Comment Update Modal ์ปดํฌ๋ํธ์์ ํ  ์ ์์ต๋๋ค. ๋๊ธ ์์ ์ ์ญ์ comment๋ฅผ props๋ก ๋ฐ์์์ผํฉ๋๋ค. ์์ ์๋ฃ๋ฅผ ๋๋ฅด๋ฉด `UPDATE_COMMENT`๊ฐ ๋์ํ๊ฒ ๋ฉ๋๋ค.

```javascript
async UPDATE_COMMENT({ commit }, comments) {
    const COMMENT_UPDATE_URL = `/api/v1/movies/comments/${comments.id}/`
    const response = await axios.put(COMMENT_UPDATE_URL,comments)
    console.log(response)
    commit('UPDATE_COMMENT')
},
```



์์ฑํ ๋๊ธ์ ํ์ธ์ ์ํด ํ๋กํ ํ์ด์ง์ ๋ค์ด๊ฐ๋ฉด,

![](./design/comment3.png)

์ด๋ฐ ํ์ด์ง๊ฐ ๋จ๊ฒ ๋๊ณ , ์ญ์ ๋ฅผ ๋๋ฅด๋ฉด ๋๊ธ์ด ์ญ์ ๋จ๊ณผ ๋์์ ๋ฆฌ์คํธ์์ ์ํ๋ ์ฌ๋ผ์ง๋๋ค.

![](./design/comment4.png)

์์ ์ ๋๋ฅด๋ฉด ์์ ๊ฐ์ ๋ชจ๋ฌ์ด ๋จ๊ฒ ๋ฉ๋๋ค.



### 5. Trail

์ํ ์์ธ ํ์ด์ง ๋ชจ๋ฌ์ ๋์์ ๋ฒํผ์ ๋๋ฅด๋ฉด Modal Trail ์ปดํฌ๋ํธ๋ก ๋ฐ๋๊ฒ ๋ฉ๋๋ค.

![](./design/trail.png)

์์ ๊ฐ์ด ์ํ ์๊ณ ํธ์ด ๋จ๋ ๊ธฐ๋ฅ์๋๋ค.



์๊ณ ํธ์ iframe์ ์ฌ์ฉํ์ฌ ๋ํ๋ด๋๋ก ํ์์ต๋๋ค. ์ ํ๋ธ์์ ์์์ ๋ฐ์์ค๊ธฐ ์ํด API key์ URL์ด ํ์ํฉ๋๋ค. ์ํ ์ ๋ชฉ์ ๊ฒ์ํ์ ๋, ์์์ ๊ฐ์ ธ์ฌ ๊ฒ์ด๋ฏ๋ก Movie data๋ฅผ props๋ก ๋ฐ์์ต๋๋ค.

๋ฐ์ดํฐ ๋ฒ ์ด์ค๋ฅผ ๋ณ๊ฒฝํ  ๊ฒ์ด ์๊ธฐ ๋๋ฌธ์ ํ์ฌ ์ปดํฌ๋ํธ์์ ํด๊ฒฐ์ด ์๋ฃ๋๋๋ก ํ์์ต๋๋ค. ์ฐ์ , ์ํ ์ ๋ชฉ์ "๊ณต์ ์๊ณ ํธ"์ด๋ผ๋ ๋จ์ด๋ฅผ ๋ถ์ฌ์ค๋๋ค. ๊ทธ๋ฆฌ๊ณ  ์ ํ๋ธ API์์ ์์ฒญํ๋ ์ ๋ณด๋ค์ ๋ด์ URL์ ๋ง๋ค์ด์ฃผ๋๋ก ํฉ๋๋ค. ์ด๋ ์ฌ์ฉํ ํจ์๊ฐ `await axios.get()`์ธ๋ฐ, ์๋์ผ๋ก ์์ฒญ URL์ ๋ด์์ค ๊ฒ์๋๋ค.

๋น๋์ค ๋ฆฌ์คํธ์ ๊ฐ์ ธ์จ ํญ๋ชฉ๋ค์ ๋ด์์ฃผ๊ณ , ์ฒซ ๋ฒ์งธ ๋์์์ ์ถ๋ ฅํ๋๋ก ํ์ต๋๋ค.

```javascript
import axios from 'axios'


const YOUTUBE_API_KEY = 'AIzaSyBCNyVF3W6jLT2rmdc5du7hHM-dXJWAoPg'
const YOUTUBE_API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default {
  name: 'ModalTrail',
  props: {
    movie: {
      type: Object,
      required: true,
    }
  },
  data(){
    return {
      videoList: ''
    }
  },

  methods: {
    async searchVideo() {
      const keyword = '๊ณต์ ์๊ณ ํธ ' + this.movie.title
      const config = {
        params: {
          part: 'snippet',
          type: 'video',
          q: keyword,
          key: YOUTUBE_API_KEY
        }
      }
      const response = await axios.get(YOUTUBE_API_URL, config)
      this.videoList = response.data.items
    },
  },
  created() {
    this.searchVideo()
  },
  computed: {

    videoUrl() {
      const videoId = this.videoList[0].id.videoId
      return `https://www.youtube.com/embed/${videoId}`
    },
  },
}
```

๋์์ ์คํ์ผ์ bootstrap์ ์ ๊ทน ์ฌ์ฉํ์์ต๋๋ค.



### 6. Profile

ํ๋กํ ํ์ด์ง์์์ ๋์๋ค์ ์๋์ ๊ฐ์ต๋๋ค.

- ๊ณ์ ์ dropdown์ ์๋ profile์ ํตํด ๋ณธ์ธ์ ํ๋กํ ํ์ด์ง๋ก, ์ด๋ํ  ์ ์์ต๋๋ค.
- ์ถ๊ฐ์ ์ผ๋ก ๋๋ค ์ถ์ฒ ๊ธฐ๋ฅ์ ๊ตฌํํ์์ต๋๋ค. ๋ฒํผ์ ๋๋ฅด๋ฉด ๋๋ค์ผ๋ก ์ฌ์ฉ์์๊ฒ ์ถ์ฒ ์ํ๋ฅผ ๋ณด์ฌ์ค๋๋ค.
- Profile ํ์ด์ง์์๋ ํด๋น ์ ์ ๊ฐ ๋ฆฌ๋ทฐ๋ฅผ ์์ฑํ ์ํ๋ค๊ณผ ๋ฆฌ๋ทฐ๋ค ์ค ๊ฐ์ฅ ์ต๊ทผ์ ์์ฑํ ๊ธ์ ๋ณด์ฌ์ค๋๋ค. ์ด๋ ์ํ ํฌ์คํฐ๋ฅผ ํด๋ฆญํ๋ฉด ํด๋น ์ํ์ ๋ชจ๋ฌ ํ์ด์ง๋ก ์ด๋ํฉ๋๋ค.

#### 01 - Refactoring

Vuex์ฑ์ ๊ตฌ์กฐ๊ฐ ํฌ๊ฒ ๋ณต์กํ์ง ์๋ค๊ณ  ํ๋จํ์ฌ `props & emit` ์ ํตํด ํ์ด์ง ํน์ ์ปดํฌ๋ํธ ๊ฐ ์ ๋ณด๋ฅผ ์ฃผ๊ณ  ๋ฐ๋๋ก ๊ตฌํํ์ต๋๋ค. 

์ ์  ๊ด๋ จ ๊ธฐ๋ฅ ์ค์ ์ฐํ ์ ๋ณด๋ฅผ ํ๋กํ ํ์ด์ง์์ ๊ตฌํํ๊ณ ์ ํ์ผ๋, ๋ชจ๋ธ ์ค์ ์ ์ ๋ชปํด์ ๊ฒฐ๊ตญ ํ์ง ๋ชปํ์ต๋๋ค. ๋ค์ ํ ๋ฒ ์ฒ์ ๋ชจ๋ธ ๊ด๊ณ ์ค์ ์ ์ ํด์ผํจ์ ๋๋ ์ ์์์ต๋๋ค. Vuex ๋ฅผ ํตํด ์ ๋ณด๋ฅผ ์ค์ํํ๋ค๋ฉด ๋ ๊ฐ๋จํ๊ฒ ์ ์ฒด ๊ธฐ๋ฅ์ ๊ตฌํํ  ์ ์์ ๊ฒ ๊ฐ์ต๋๋ค.



### 7. Recommend

์ํ ์ถ์ฒ์ ์ฌ์ฉ์๊ฐ ์ฃผ๋ก ์ข์ํ๋ ์ํ ์ฅ๋ฅด๋ฅผ ์์ฃผ๋ก ์ถ์ฒํด์ฃผ๋ ํ๋ฆ์ผ๋ก ๊ตฌ์ฑํ๊ณ  ์ฝ๋๋ฅผ ์์ฑํ์ต๋๋ค.

์ด๋ฅผ ์ํด ๊ฐ ์ฅ๋ฅด์ ์นดํ๊ณ ๋ฆฌ๋ฅผ ๊ฐ๋ ํ์ด๋ธ์ ๋ง๋ค์๊ณ  ์ ์ ์ ์ธ๋ํค ๊ด๊ณ๋ฅผ ์ง์ด์ฃผ์์ต๋๋ค.

1. ์ฌ์ฉ์๊ฐ ๊ฐ๋ช๊น๊ฒ ๋ณธ ์ํ๋ค์ ์๋ ฅ๋ฐ์์ ํด๋น ์ํ์ ์ฅ๋ฅด์ ๋ฐ๋ผ ์ฌ์ฉ์์ ์ฅ๋ฅด ์ ์๋ฅผ ์ฌ๋ฆฝ๋๋ค.
   (ex. ์ก์์ํ์๋ค๋ฉด ์ก์์ฅ๋ฅด ์ ์ ++)
2. ์ ์๋ฅผ ๊ธฐ์ค์ผ๋ก ์์ 3๊ฐ์ ์ฅ๋ฅด์ ํด๋นํ๋ ์ํ๋ฅผ ๋๋ค์ผ๋ก 8๊ฐ ์ถ์ฒํฉ๋๋ค.

์ด๋ ๊ฒ ๋ ๊ณผ์ ์ ๊ฑฐ์ณ ๋๋ง์ ์ถ์ฒ ์๊ณ ๋ฆฌ์ฆ์ ๋ง๋ค์์ต๋๋ค.

```html
  <div>
    <div class="col my-4" id="">
      <div class="movie" @click="chkMovie">
        <img 
          :class="{ userlike: myMovie.includes(movie.id) }" 
          class="w-75 border" 
          :src="movie.poster" 
          alt=""
        >
        <span class="mt-3">{{ movie.title }}<i class="bi bi-bookmark-plus"></i></span>
      </div>
    </div>
  </div>
...
<script>
    ...
  methods: {
    chkMovie() {
      const movie_id = this.movie.id
      this.$store.dispatch('CHECK_MOVIE', movie_id)
      this.$store.dispatch('GET_MY_MOVIE_LIST')
    },
  }
</script>
```

๊ธฐ๋ณธ์ ์ผ๋ก ์ ์ฒด ์ํ ๋ฆฌ์คํธ๋ฅผ V-for๋ฌธ์ผ๋ก ๋๋ ธ์ต๋๋ค. (์ ์ฝ๋๋ V-for๋ฌธ ๋๋ฆฌ๋ ์์ ์ปดํฌ๋ํธ)

๊ทธ๋ฆฌ๊ณ  ๊ฐ ์ํ์ ํด๋ฆญ์ด๋ฒคํธ๋ฅผ ๊ฑธ์ด์ ๋๋ฅด๋ฉด `CHECK_MOVIE`๋ผ๋ actions๊ฐ ์คํ๋ฉ๋๋ค.

```js
actions: {
    ...
    async CHECK_MOVIE({commit}, movie_id) {
      const CHECK_MOVIE_URL = '/api/v1/movies/checklist/'
      
      const data = {
        'username': localStorage.getItem('username'),
        'movie': movie_id
      }
      const response = await axios.post(CHECK_MOVIE_URL, data)
      
      commit('CHECK_MOVIE', response.data)
    },
}
```

actions๊ฐ ์คํ๋๋ฉด ๋ฏธ๋ฆฌ ๊ฑธ์ด๋ URL๋ก Django ์๋ฒ๋ก ์์ฒญ์ด ๊ฐ๊ฒ ๋ฉ๋๋ค.

๋ฐฑ์๋์์ ํ์ํ ๋ฐ์ดํฐ๋ฅผ ๋ด์์ axios์์ฒญ์ ๋ณด๋์ต๋๋ค.

Django์๋ฒ์์๋ ์ ์ ์ ์ํ ๋ฐ์ดํฐ๋ฅผ ๋ฐ๊ณ , ์ฌ์ฉ์์ ํ์ด๋ธ์ ํด๋น ์ฌ์ฉ์๊ฐ ํด๋น ์ํ๋ฅผ ์ข์ํ๋ค๋ ๋ด์ฉ์ ์ ์ฅํ์ต๋๋ค.(์ข์์์ ๊ฐ์ ๊ธฐ๋ฅ)

```js
mutations: {
    ...
	CHECK_MOVIE(state, payload) {
      state.myMovieList.push(payload)
    }
}
```

๊ทธ๋ฆฌ๊ณ  mutations์์ ์ฒดํฌํ ์ํ ์ ๋ณด๋ฅผ myMovieList๋ผ๋ state์ ๊ธฐ๋กํด ๋์์ต๋๋ค.

๊ทธ๋ฆฌ๊ณ  `EvalMovieListItem.vue`์์ `myMovieList`์ ์ํด์๋ ์ํ๋ค๋ง cssํจ๊ณผ๋ฅผ ์ฃผ์ด ๋ค๋ฅธ ์ํ๋ค๊ณผ ๊ตฌ๋ถ์ง์ด ์ฃผ์์ต๋๋ค.

์ด ๊ณผ์ ์ ํตํด์ ์ฌ์ฉ์๊ฐ ๊ฐ๋ช๊น๊ฒ ๋ณธ ์ํ ํน์ ์ข์ํ ์ํ๋ค์ ํ์ด๋ธ์ ์๋ ฅ๋ฐ์์ต๋๋ค.

๋ค์ ๊ณผ์ ์์๋ ๊ทธ ์ํ๋ค์ ์ฅ๋ฅด๋ค์ ๊ธฐ๋ฐ์ผ๋ก ์ฌ์ฉ์์ ์ฅ๋ฅด๋ณ ์ ์๋ฅผ ์ฃผ๊ณ  ์์ 3๊ฐ ์ฅ๋ฅด๋ฅผ ๊ณจ๋ผ ์ํ์ถ์ฒ์ ํด์ฃผ์์ต๋๋ค.

```python
 for genre in genres:
            if genre == '์ก์':
                user_data.action = user_data.action + 1
                user_data.save()
            if genre == '๋ชจํ':
                user_data.adventure = user_data.adventure + 1
                user_data.save()
            if genre == '์ ๋๋ฉ์ด์':
                user_data.animation = user_data.animation + 1
                user_data.save()
            .....
```

์ด๋ฐ์์ผ๋ก ์ฌ์ฉ์๊ฐ ์ฒดํฌํ๋(๊ฐ๋ช๊น๊ฒ ๋ดค๋)์ํ์ ์ฅ๋ฅด์ ๋ฐ๋ผ ์ ์๋ฅผ 1์ ์ฉ ์ฃผ์์ต๋๋ค.

์ก์์ ๋ง์ด ์ฒดํฌํ๋ค๋ฉด ์ก์์ํ๋ฅผ ๋ ์ถ์ฒ๋ฐ๋ ๋๋์ด ๋ค ๊ฒ์๋๋ค.



## ๐Documents

### ๐Docs

- [Getting Started | BootstrapVue (bootstrap-vue.org)](https://bootstrap-vue.org/docs)
- [.data() | jQuery API Documentation](https://api.jquery.com/data/)
- [Bootstrap ยท The most popular HTML, CSS, and JS library in the world. (getbootstrap.com)](https://getbootstrap.com/)
- https://github.com/surmon-china/vue-awesome-swiper
- [๋กฏ๋ฐ์๋ค๋ง](lottecinema.co.kr)



### ๐ฌIssues

#### Timeline

> ๊ฐ๋ฐ ๊ธฐ๊ฐ: 20/05/2021 ~ 28/05/2021



#### Contributor

| ์ด๋ฆ   | ์ญํ ๋ถ๋ด           |
| ------ | ------------------ |
| ๊ณ ์ ์ | FrontEnd repo      |
| ๋ฐํจ์ง | BackEnd repo       |
| ์ต์ฐฝ์ | FrontEnd + BackEnd |
