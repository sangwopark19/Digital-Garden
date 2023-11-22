zsh에 추가기능을 넣은 더 강력한 쉘

## 버그
---
- 붙여넣기가 느려지고, 한줄씩 입력되는 현상, \ back slash가 붙는 현상: 버그는 아니고 ~/.zshrc 파일에서 `DISABLE_MAGIC_FUNCTIONS=true`주석을 해제해주면 된다. 
	- 참조: https://github.com/ohmyzsh/ohmyzsh/issues/5569