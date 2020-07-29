# Cross Code 기술 블로그 소스

- 포스트 작성 후 배포 하면 아래 도메인에 적용됩니다.
- https://cross-code.github.io

## 함께 만들어가는 기술 블로그

글을 작성하고 공유함으로부터 배우는 것이 분명히 있다고 생각합니다.<br>
관심 있고 공부하고 싶은 내용이나 알고 있는 내용을 하나씩 담아 보았으면 합니다.

작성하신 내용은 물론 개인 블로그에 올리셔도 됩니다.<br>
개인 블로그에 작성하신 내용을 반대로 이곳에 작성해주셔도 좋습니다.<br>
여러 사람이 함께 사용하는 블로그이니 문서 하단에 꼭 작성자를 남겨주세요.

## Hexo CLI 설치

```bash
$ npm install -g hexo-cli
```

## 블로그 서버 실행

```bash
$ npm run start
```

More info: [Server](https://hexo.io/docs/server.html)

## 새로운 포스트 생성

```bash
// 일반적인 포스트 작성
$ hexo new "My New Post"

// draft 포스트 작성 (배포해도 remote 서버에 반영되지 않는 문서)
$ hexo new draft "My New Post"

// draft 포스트 배포에 포함시키기
$ hexo publish post "Post Name"
```

More info: [Writing](https://hexo.io/docs/writing.html)

## 빌드와 배포를 한번에 (cross-code.github.io에 배포됩니다)

```bash
$ npm run deploy
```

## 커밋 코멘트 작성룰

- chore: Other changes that don't modify src or test files
- fix: A bug fix
- feat: A new feature
- docs: Documentation changes
- test: Adding missing tests or correcting existing tests
- build: Changes that affect the build system
- ci: Changes to our CI configuration files and scripts
- perf: A code change that improves performance
- refactor: A code change that neither fixes a bug nor adds a feature
- style: Changes that do not affect the meaning of the code (linting)
- vendor: Bumping a dependency like libchromiumcontent or node

## 참고

- [Markdown 작성법](https://gist.github.com/ihoneymon/652be052a0727ad59601)
- [Hexo 공식 문서](https://hexo.io/ko/docs)
- [Hexo 기본 사용법](https://futurecreator.github.io/2016/06/21/hexo-basic-usage/)
- [이모지](https://getemoji.com)


## 괜찮은 블로그 테마 찾기

- https://github.com/next-theme/hexo-theme-next (사용중)
- https://blog.zhangruipeng.me/hexo-theme-hueman
- https://blog.zhangruipeng.me/hexo-theme-icarus


