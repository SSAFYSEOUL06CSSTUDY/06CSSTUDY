# Swagger란?
> REST API를 설계, 빌드, 문서화 및 사용하는 데 도움이되는 OpenAPI 사양을 중심으로 구축 된 오픈 소스 도구 세트
- Springboot에서 Swagger를 사용하면, 컨트롤러에 명시된 어노테이션을 해석하여 API문서를 자동으로 만들어준다.
- API 목록 뿐만 아니라 API의 명세 및 설명도 볼 수 있으며, API를 직접 테스트해 볼 도 있다.
- Swagger는 Java에 종속된 라이브러리가 아니다.
- API 페이지 호출 http://localhost:8080/swagger-ui/index.html

# Swagger 설정
- build.gradle에 의존성 추가
```
// swagger
compile 'io.springfox:springfox-swagger2:2.9.2'
compile 'io.springfox:springfox-swagger-ui:2.9.2'
```
- pom.xml에 swagger2 dependency 추가
```
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>3.0.0</version>
</dependency>
<dependency>
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>3.0.0</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-boot-starter</artifactId>
	<version>3.0.0</version>
</dependency>
```
# SwaggerConfig 클래스
```java
package com.ssafy.board.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

@Configuration
//@EnableSwagger2
public class SwaggerConfig {
	@Bean
	public Docket api() {
		return new Docket(DocumentationType.SWAGGER_2)
				//.useDefaultResponseMessages(false)
                		//.groupName("groupName2")
				.select()
				.apis(RequestHandlerSelectors.basePackage("com.ssafy.board.controller"))
				.paths(PathSelectors.ant("/api*/**"))
				.build()
				.apiInfo(apiInfo());
	}
	
	private ApiInfo apiInfo() {
		return new ApiInfoBuilder()
				.title("SSAFY 프로젝트")
				.description("정말 멋진 사이트야.")
				.version("0.1")
				.build();
	}
}
```
### @EnableSwagger2
- `Swagger2` 버전 활성화하는 어노테이션
### Docket
- `Swagger` 설정을 할 수 있게 도와주는 클래스
- API 자체에 대한 정보는 컨트롤러에서 작성
### useDefaultResponseMessages()
- false로 설정하면 Swagger에서 제공해주는 응답코드(200, 401, 403, 404)에 대한 기본 메시지를 제거해준다.
### groupName()
- Docket Bean이 한 개일 경우 생략해도 상관없으나, 둘 이상일 경우 충돌 방지를 위해 설정해줘야 한다.
### select()
- ApiSelectorBuilder를 생성하여 apis()와 paths()를 사용할 수 있게 해준다.
### apis()
- api 스펙이 작성되어 있는 패키지를 지정한다.
- 즉, 컨트롤러가 존재하는 패키지를 basepackage로 지정하여 해당 패키지에 존재하는 API를 문서화 한다.
### paths()
- apis()로 선택되어진 API중 특정 path 조건에 맞는 API들을 다시 필터링하여 문서화한다.
- PathSelectors.any()로 설정하면 패키지 안에 모든 API를 한 번에 볼 수 있다.
### apiInfo()
제목, 설명 등 문서에 대한 정보들을 설정하기 위해 호출한다.

# Controller
```java
@RestController
@RequestMapping("/api)
@Api("게시판 컨트롤러")
public class BoardRestController {
	@GetMapping("/board")
	@ApiOperation(value="게시글 조회", notes="검색조건도 넣으면 같이 가져온다.")
	public ResponseEntity<?> list(SearchCondition condition){
```
### @Api
- 해당 클래스가 Swagger 리소스라는 것을 명시해준다.
	- value : 사용자 지정 이름을 붙일 수 있다. tags 사용시 무시된다.
	- tags : 사용하여 여러 개의 태그를 정의할 수도 있다.
### @ApiOperation
- 한 개의 Operation을 선언한다.
	- value : API에 대한 요약을 작성한다. 제대로 표시되려면 120자 이하여야 한다.
	- notes : API에 대한 자세한 설명을 작성한다.
### @ApiParam
- 파라미터에 대한 정보를 명시한다.
	- name : 파라미터 이름을 작성한다.
	- value : 파라미터 설명을 작성한다.
	- required : 필수 파라미터이면 true, 아니면 false를 작성한다.
 
![swagger 화면](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/50236187/b1199431-08d9-4ec9-b24a-f4d3e323b1ef)
