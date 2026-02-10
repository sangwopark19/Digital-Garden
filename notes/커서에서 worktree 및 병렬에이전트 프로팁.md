  
커서에서 워크트리 관리와 활용을 위한 전문가 팁입니다. 베스트 오브 엔과 워크트리가 제가 지금 커서를 사용하는 주된 이유입니다.

전문가 팁:
- 채팅 및 서브 컴포저 탭 순환 단축키 각각 할당하기(수정됐다면)
- 변경 사항 적용 단축키 할당하기
- 변경 사항 저장/복원 단축키 할당하기

변경 사항 저장/병합/덮어쓰기 대화상자 우회 단축키 또는 매크로(아리아 레이블 미지원으로 인해 홈 행 키로 이동 후 클릭하거나 탭 키 사용 불가. 몇 주째 수정 요청 중이니 커서 개발자 중 파워 유저가 있다면 탭/스크린 리더로 대화상자 제어 기능을 추가해 주세요. 이 기능이 없다는 건 끔찍한 사용자 경험입니다)

—-

여러 에이전트의 답변을 검토해야 하는 병목 현상이 발생하므로, 답변 검토 속도를 높이는 것이 더 효과적입니다. 따라서 제 조언은 변경 사항을 더 빠르게 순환하고 적용할 수 있는 방법에 집중됩니다.

새 워크트리 작성기 단축키 설정: VS Code의 'runCommands' 명령어를 활용해 새 워크트리 채팅을 엽니다. 간편한 방법은 없지만 키 바인딩 설정에서 세 가지 단축키를 조합해 구현할 수 있습니다.

—-

커서 팀을 위해

- 추가 채팅 창은 이후 열릴수록 느려짐
    

- 단일 단축키로 사전 설정된 '모드'와 '다중 모델 선택'이 적용된 컴포저를 열 수 있는 방법이 필요합니다. 해결해야 할 작업마다 필요한 모델이 다릅니다. 매번 메뉴를 수동으로 거쳐 실행할 다중 모델을 변경해야 하는 것은 개발자 경험(DX)을 저해합니다. 사용자 정의 모드를 제거했지만, 이전의 작은 하위 메뉴보다 더 나은 UI로 설정할 수 있도록 다시 도입할 수 있습니다.

- 작문기 위치를 순환할 때 클라우드 순환을 비활성화할 수 있게 해주세요. 그렇게 하면 순환할 때 이 옵션이 표시되지 않습니다 (제가 까다롭게 구는 거죠, 하하).

Here are my pro tips to manage worktrees and get the most out of them in cursor. Best of n and worktrees are the key reason I’m using cursor right now.

Pro tips: Bind each of the shortcuts for cycling through chats and sub composer tabs(if they fixed it)

Bind shortcuts to apply changes

Shortcuts to stash /pop changes

Shortcut or macro to bypass the stash / merge/ overwrite dialog(no aria label support means you can’t use home row to jump and click it or tab, I’ve been asking for them to fix this for weeks now, so if there is a dev at cursor who actually is a power user please add support to control the dialogs with tab / screen reader, it’s terrible dx that we can’t do that)

—-

The bottleneck you’ll experience is that you have to review multiple agent answers so being able to review those answers faster is better. Hence why the my advice revolves around being able to cycle and apply changes faster.

Set up a new worktrees composer shortcut Use vs code ‘runCommands’ commands to open a new worktree chat, there is no easy way to do this but you can combine three shortcuts to do this in the keybindings config.

—-

For the team at cursor

- more chats slow down on subsequent open
    

-we need a way to press one shortcut and open a composer with preset ‘mode’ , ‘multiple model selections’ there are different tasks that require different models to solve . It kills dx that we have to manually go through the menus every time to change the multiple models we want to run with.you removed custom modes but you could bring it back with a better ui to set it up than that small sub menu you had before.

- let us disable cycling to the cloud when we cycle composer location. That way when we cycle we don’t see this option (just me being nit picky, ha)