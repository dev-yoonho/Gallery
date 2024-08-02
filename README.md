# SSAFIT 프로젝트

이 프로젝트는 JSON 형태의 영상 정보를 파싱하고, 목록과 리뷰를 관리하는 프로그램입니다. 회원 가입 및 로그인을 포함한 회원 관리 기능도 제공합니다.

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![Version](https://img.shields.io/badge/version-1.0.0-blue)

## 목차
1. [프로젝트 소개](#프로젝트-소개)
2. [설치 방법](#설치-방법)
3. [사용법](#사용법)
4. [클래스 다이어그램 변화](#클래스-다이어그램-변화)
5. [코드 변화](#코드-변화)
6. [기타](#기타)

## 프로젝트 소개

이 프로젝트는 다음과 같은 기능을 제공합니다:

### 기본 기능
1. 제공되는 영상 정보 데이터 파일을 기반으로 필요한 정보를 파싱 처리
2. 파싱 처리 후 영상 정보를 제공하는 목록 화면과 영상에 대한 리뷰를 관리하는 프로그램 구현
3. 데이터 파일은 JSON 형태로 제공
4. 데이터 처리는 Gson 라이브러리를 사용
5. 영상에 대한 리뷰 관리 프로그램은 영상 목록 화면에서 영상을 선택한 후 선택한 영상과 관련된 리뷰 목록, 등록 기능 제공

### 추가 기능
1. 회원가입 및 가입한 회원 정보를 보여주는 목록 화면 구현
2. 프로그램 종료 시 가입한 회원의 정보를 파일로 저장하고 프로그램 시작 시 파일에 저장되어 있는 회원의 정보를 로딩
3. 회원 정보는 아이디, 이름, 비밀번호, 이메일을 필수로 하고 이외의 정보들은 필요에 따라서 추가

### 심화 기능
1. 로그인, 로그아웃 기능을 구현
2. 로그인과 로그아웃 기능은 추가 기능에서 구현한 회원 정보를 이용하여 자유롭게 구현

## 설치 방법

1. 리포지토리를 클론합니다:
    ```sh
    git clone https://github.com/your-repo/ssafit-project.git
    ```
2. 필요한 라이브러리를 설치합니다:
    ```sh
    cd ssafit-project
    mvn install
    ```

## 사용법

1. 프로그램을 실행합니다:
    ```sh
    mvn exec:java -Dexec.mainClass="com.ssafit.SsafitApplication"
    ```
2. 주요 기능 사용 방법:
    - 영상 목록 보기
    - 영상 리뷰 등록 및 보기
    - 회원 가입 및 로그인

## 클래스 다이어그램 변화
**초기**
![초기 클래스 다이어그램](https://github.com/user-attachments/assets/2aa554f3-202a-4c26-a9af-7903bfb84e12)
**중기** 
![중기 클래스 다이어그램](https://github.com/user-attachments/assets/503e7648-fd95-4d9b-a6ab-e94e8ec03262)
**말기**
![말기 클래스 다이어그램](https://github.com/user-attachments/assets/b174944c-486d-4a49-81aa-c5517828be5a)

### 초기 -> 중기

#### 변화된 점
1. **VideoDao 인터페이스 변경**:
   - `selectVideo()`와 `selectVideoByNo(int no)` 메서드가 `getAllVideos()`와 `getVideoByNo(int no)`로 변경되어 더 직관적인 메서드 명을 사용하게 됨.
2. **VideoUi 클래스 변경**:
   - 싱글톤 패턴 제거: `VideoUi` 클래스에서 `getInstance()` 메서드를 사용한 싱글톤 패턴이 제거되고, 생성자 주입 방식으로 `VideoDao` 객체를 받도록 변경.
3. **VideoReviewDao 인터페이스 변경**:
   - `inserReview(videoReview: VideoReview): int`와 `selectReview(videoNo: int): List<VideoReview>` 메서드가 각각 `insertReview(videoReview: VideoReview): int`와 `getReviewsByVideoNo(videoNo: int): List<VideoReview>`로 변경되어 명명 규칙이 개선됨.
4. **VideoReviewDaoImpl 클래스 변경**:
   - `VideoReviewDao` 인터페이스를 명시적으로 구현하도록 변경되어 코드의 가독성과 유지보수성이 향상됨.
5. **기타**:
   - 클래스와 인터페이스 간의 관계가 명확하게 정의되고, 전반적인 코드 구조가 더 일관성 있게 정리됨.

### 중기 -> 말기

#### 변화된 점
1. **UserDao 인터페이스 추가**:
   - 회원 정보를 관리하는 `UserDao` 인터페이스가 추가되어, `saveUsersToFile()`, `loadUsersFromFile()`, `addUser(User user)`, `getUserById(String id)`, `getAllUsers()` 메서드를 정의.
2. **User 클래스 추가**:
   - 회원 정보를 담는 `User` 클래스가 추가되어, 아이디, 이름, 비밀번호, 이메일, 추가 정보 필드를 포함한 객체 구조를 정의.
3. **UserDaoImpl 클래스 추가**:
   - `UserDao` 인터페이스를 구현하는 `UserDaoImpl` 클래스가 추가되어, 사용자 정보를 파일에 저장하고 로드하는 기능을 구현.
4. **UserUi 클래스 추가**:
   - 회원 가입, 로그인, 로그아웃, 사용자 목록 표시 등의 기능을 제공하는 `UserUi` 클래스가 추가됨.
5. **MainUi 클래스 변경**:
   - `MainUi` 클래스에서 `UserUi` 객체를 관리하도록 변경되어, 사용자 인터페이스 관련 기능이 통합됨.
6. **기타**:
   - 클래스와 인터페이스 간의 관계가 더 복잡해지고 명확해짐으로써, 기능이 확장되고 유지보수성이 향상됨.

## 코드 변화

### 초기 코드
```java
class VideoUi {
    private VideoDao videoDao = VideoDaoImpl.getInstance();
    private static VideoUi instance = new VideoUi();
    private VideoUi() {}
    public static VideoUi getInstance() {
        return instance;
    }
    public void service() {
        // ... (기타 코드)
    }
    private void listVideo() {
        // ... (기타 코드)
    }
    private void detailVideo() {
        // ... (기타 코드)
    }
}
```

### 개선된 코드
```java
class VideoUi {
    private VideoDao videoDao;
    public VideoUi(VideoDao videoDao) {
        this.videoDao = videoDao;
    }
    public void service() {
        // ... (기타 코드)
    }
    private void listVideo() {
        // ... (기타 코드)
    }
    private void detailVideo() {
        // ... (기타 코드)
    }
}
```

### 변화 및 개선된 점 요약
- **의존성 주입 사용**: `VideoUi` 클래스의 생성자에서 `VideoDao` 객체를 주입받도록 하여 테스트 용이성과 객체 간 결합도를 낮춤.
- **싱글톤 패턴 제거**: 불필요한 싱글톤 패턴을 제거하여 객체 생성의 유연성을 증가시킴.

## 기타

### 릴리즈 노트
- **v1.0.0**: 최초 릴리즈

### 저자 및 기여자 목록
- 김범준
- 조윤호

### 사용된 기술 스택
- Java
- Maven
- Gson

### 감사 인사
프로젝트에 기여해주신 모든 분들께 감사드립니다.
