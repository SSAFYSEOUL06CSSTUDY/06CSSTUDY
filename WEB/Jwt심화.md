## 목차

1. [**프로젝트에 JWT 적용**](#1-프로젝트에-jwt-적용)
2. [**Jwt의 문제점**](#2-jwt의-문제점)
3. [**Refresh Token의 필요성**](#3-refresh-token의-필요성)
4. [**Refresh Token을 프로젝트에 적용**](#4-refresh-token을-프로젝트에-적용)
5. [**Refresh Token을 DB에 저장하는 이유**](#5-refresh-token을-db에-저장하는-이유)
6. [**Token의 유효성 검증**](#6-token의-유효성-검증)

### 1. 프로젝트에 JWT 적용

#### - 의존성 추가

2가지 라이브러리 중 택 1

[io.jsonwebtoken:jjwt]

> 가장 대표적인 Java - Jwt 라이브러리

```
<dependency>
			<groupId>io.jsonwebtoken</groupId>
			<artifactId>jjwt</artifactId>
			<version>Ver?</version>
</dependency>
```

[com.auth0:java-jwt]

> Oauth 인증과 쉽게 연동 할 수 있게 설정된 라이브러리<br>
> jjwt와 다르게 key 없이도 디코딩 가능

```
<dependency>
   <groupId>com.auth0</groupId>
   <artifactId>java-jwt</artifactId>
   <version>Ver?</version>
</dependency>

```

#### - Spring Boot적용

<details>
<summary>Config 설정</summary>

```
import io.jsonwebtoken.*;
import java.util.Date;

public class JwtTokenProvider {
    private String secretKey = "사용할 시크릿 키"; // 시크릿 키 설정(유출 되면 안됨)
    private long validityInMilliseconds = 3600000; // 토큰 유효 시간 설정 (예: 1시간)

    public String createToken(String username) {
        Claims claims = Jwts.claims().setSubject(username);
        Date now = new Date();
        Date validity = new Date(now.getTime() + validityInMilliseconds);

        return Jwts.builder()
                .setClaims(claims)
                .setIssuedAt(now)
                .setExpiration(validity)
                .signWith(SignatureAlgorithm.HS256, secretKey)
                .compact();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token);
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            throw new CustomException("Expired or invalid JWT token", HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }

    public String getUsername(String token) {
        return Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token).getBody().getSubject();
    }
}

```

</details>
<details>
<summary>Interceptor 설정</summary>

```
import org.springframework.web.filter.GenericFilterBean;
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class JwtTokenFilter extends GenericFilterBean {

    private JwtTokenProvider jwtTokenProvider;

    public JwtTokenFilter(JwtTokenProvider jwtTokenProvider) {
        this.jwtTokenProvider = jwtTokenProvider;
    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain filterChain)
            throws IOException, ServletException {
        String token = resolveToken((HttpServletRequest) req);
        if (token != null && jwtTokenProvider.validateToken(token)) {
            filterChain.doFilter(req, res);
        } else {
            HttpServletResponse response = (HttpServletResponse) res;
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        }
    }

    private String resolveToken(HttpServletRequest req){
        String bearerToken = req.getHeader("Authorization");
        if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}

```

</details>
<details>
<summary>토큰 발급</summary>

```
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginData loginData) {
    // 여기에 로그인 서비스 로직 작동
    String token = jwtTokenProvider.createToken(username);
    return ResponseEntity.ok().body(new AuthToken(token));
}

```

</details>
<details>
<summary>컨트롤러에서 토큰 검증</summary>

```
@RestController
@RequestMapping("/api")
public class UserController {

    @GetMapping("/user/{username}")
    public ResponseEntity<?> getUser(@PathVariable String username) {
        // 사용자 정보 조회 서비스 로직 호출
        return ResponseEntity.ok().body(user);
    }
}

```

</details>

#### - Vue3.js 적용

<details>
<summary>로그인 시 토큰 저장</summary>

```
axios.post('/api/login', { username, password })
    .then(response => {
        localStorage.setItem('token', response.data.token);
    });
```

</details>
<details>
<summary>JWT Interseptor(axios)</summary>

```
axios.interceptors.request.use(config => {
    const token = localStorage.getItem('token');
    if (token) {
        config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
});
```

</details>

#### - 전체적인 JWT 로직 흐름도

<!-- JWT logic flow img -->

### 2. Jwt의 문제점

> 위의 실제 프로젝트에 JWT를 적용하면서 알 수 있는 문제점 들을 고민해 봅니다.

- 상태 저장을 하지않음
  > 무상태(stateless) 토큰 이므로 세션방식과 다르게 서버에서 사용자의 상태를 모니터링(관리)하는 것이 불가능
- 토큰 탈취 & 긴 유효 기간 문제점
  > 코드를 통해 알 수 있드시 유효한 토큰이라면 탈취해서 다른 클라이언트 에서 사용한다고 하더라도 유효하게 사용가능, JWT만 단독 사용시 반복되는 로그인&로그아웃을 방지 하기 위해 긴 토큰 유효시간을 설정하므로 토큰 탈취의 위험도 또한 증가

### 3. Refresh Token의 필요성

> Jwt의 문제점은 Refresh Token의 도입을 통해 해결 가능

- Access Token 과 Refresh Token
  > 위에서 직접적인 인증에 사용했던 토큰은 이제 Access Token으로 Access Token의 재발급에 사용되는 토큰은 Refresh Token으로 호칭
- JWT의 짧은 유효기간 및 재발급
  > Access Token은 이제 매우 짧은 유효기간을 가지며, 주기적으로 Refresh Token을 통해 새로 발급됨으로서 탈취를 예방

### 4. Refresh Token을 프로젝트에 적용

#### - Spring Boot

<details>
<summary>JWT Config에 적용</summary>

```
@public class JwtTokenProvider {
    // 기존 JWT 관련 코드는 생략

    public String createRefreshToken() {
        // Refresh Token 생성 로직
        Date now = new Date();
        Date validity = new Date(now.getTime() + refreshValidityInMilliseconds);
        // 이 생성 로직은 그냥 예시
        return Jwts.builder()
                .setIssuedAt(now)
                .setExpiration(validity)
                .signWith(SignatureAlgorithm.HS256, secretKey)
                .compact();
    }
}

```

</details>
<details>
<summary>컨트롤러에 적용</summary>

> 이부분에서 Refresh Token의 차이가 생깁니다.<br>
> 일반 적으로 Refesh Token은 DB에 저장해 관리합니다.

```
@RestController
public class AuthenticationController {

    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginData loginData) {
        // 로그인 로직 및 JWT 로직은 생력

        //Refresh Token 생성
        String refreshToken = jwtTokenProvider.createRefreshToken();

        // Refresh Token 데이터베이스에 저장
        refreshTokenService.save(new RefreshToken(username, refreshToken));

        // JWT 및 Refresh Token 반환
        return ResponseEntity.ok(new AuthenticationResponse(jwt, refreshToken));
    }

}

```

> Refresh Token 을 위한 DTO

```
public class RefreshToken {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String username;
    private String token;
    private Instant expiryDate;
    ...
}

```

</details>
<details>
<summary>Access Token 재발급</summary>

```
@RestController
public class TokenController {

    @PostMapping("/token/refresh")
    public ResponseEntity<?> refreshAccessToken(@RequestBody TokenRefreshRequest request) {
        String requestRefreshToken = request.getRefreshToken();

        // Refresh Token 검증 및 사용자 확인
        // 데이터베이스에서 Refresh Token 확인
        RefreshToken refreshToken = refreshTokenService.findByToken(requestRefreshToken)
                .orElseThrow(() -> new TokenRefreshException(requestRefreshToken, "Refresh token is not in database!"));

        // 새로운 JWT 발급
        String jwt = jwtTokenProvider.createToken(refreshToken.getUsername());

        // 새로운 Refresh Token 생성 및 저장
        String newRefreshToken = jwtTokenProvider.createRefreshToken();
        refreshToken.updateToken(newRefreshToken);

        // 새로운 JWT 및 Refresh Token 반환
        return ResponseEntity.ok(new AuthenticationResponse(jwt, newRefreshToken));
    }
}



```

</details>

#### - Vue3

<details>
<summary>Client Token 저장(Vue3)</summary>

> 이전 코드와 큰 차이는 없습니다.

```
axios.post('/api/login', { username, password })
    .then(response => {
        localStorage.setItem('accessToken', response.data.accessToken);
        localStorage.setItem('refreshToken', response.data.refreshToken);
    });

```

</details>
<details>
<summary>Access Token 만료시 갱신</summary>

```
axios.interceptors.request.use(
    config => {
        const token = localStorage.getItem('accessToken');
        if (token) {
            config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
    },
    error => {
        return Promise.reject(error);
    }
);

axios.interceptors.response.use(
    response => response,
    async error => {
        const originalRequest = error.config;
        if (error.response.status === 401 && !originalRequest._retry) {
            originalRequest._retry = true;
            const refreshToken = localStorage.getItem('refreshToken');
            const res = await axios.post('/api/token/refresh', { refreshToken });

            if (res.status === 200) {
                localStorage.setItem('accessToken', res.data.accessToken);
                axios.defaults.headers.common['Authorization'] = `Bearer ${res.data.accessToken}`;
                return axios(originalRequest);
            }
        }
        return Promise.reject(error);
    }
);

```

</details>

### 5. Refresh Token을 DB에 저장하는 이유

> Refresh Token 또한 기본적으로 stateless 방식으로 토큰을 관리하기 때문에 탈취의 위험성이 있습니다.<br>따라서 DB에 저장해 관리함으로서 세션 방식과 유사하게 stateful하게 관리하는 방식을 주로 사용합니다.

> DB에 Refresh Token을 저장해서 관리함으로서 아래과 같은 보완 관리 방식을 사용할 수 있습니다.

- 복수기기 관리

  > 동일 계정으로 로그인할 수 있는 클라이언트의 갯수를 제한 할 수 있습니다. 유효한, 유효하지 않은 클라이언트의 Token을 구분해서 관리가 가능 ex) Netflix

- 중복 로그인 방지

  > 가장 최근에 발급된 Refresh Token만 DB에서 유효하게 관리하게 함으로서 중복 접속을 차단 ex) 온라인 시험

- 토큰 무효화
  > DB에서 토큰을 관리 할 경우 이전 방식과 다르게 서버에서 토큰을 차단해 접속을 원천적으로 막을 수 있음

> Token을 DB에 저장함으로서 세션 방식과 유사해지는 부분이 발생, 이 부분에 대한 고민이 필요

### 6. Token의 유효성 검증

> JWT로 유저 A가 유저 B가 작성한 게시글을 수정 할려고 할때 서버에서 이를 어떻게 처리 할까요?

> 일반적으로 토큰을 복호화 한후 토큰의 ID와 DB의 ID를 대조해 처리 합니다.

> 이 경우 토큰 탈취가 될 경우는 결국 해결하지 못 합니다.

> 이를 보완 하기 위해 크게 2가지 보완 방식이 있습니다.

1. Session 방식과 병행해서 사용
   > 안정적이지만 서버에 가해지는 부하가 크기에 제한적으로만 사용 가능
2. HTTPS 사용
   > 암호환된 채널인 HTTPS를 사용해 통신 중의 탈취를 방지
