# 2D_CampusLife
 
졸업작품_캡스톤 디자인 동상

1.메인화면

![image](https://user-images.githubusercontent.com/48191157/71571288-e76d4c80-2b1c-11ea-9eb2-7bc3216c305e.png)

2.학교에 대한 정보_1

![image](https://user-images.githubusercontent.com/48191157/71571306-fe13a380-2b1c-11ea-841f-24e11aaccfce.png)

2_1.조이스틱 조작

![image](https://user-images.githubusercontent.com/48191157/71571319-0835a200-2b1d-11ea-8afe-b9f99bc8c171.png)

    public JoyStick joystick;
    public GameObject _Joypad;  //? 조이패드
    public bool is_Joystick = true;
    //? 플레이어 이동속도
    public float Speed { get { return speed; } set { speed = value; } }
    float vertical, horizontal;
    bool is_canMove = true;            //? 이동가능?
    float speed;
    float runspeed;
    int walkCount = 2;
    int currentWalkCount = 0;
    Vector2 _PlayerVector;
    //? 플레이어 컴퍼넌트
    BoxCollider2D _PlayerBoxColider2D;
    [SerializeField] LayerMask _PlayerLayMask;
    Animator _PlayerAnimator;
    private void Awake()
    {          

    }
    private void Start()
    {
        //!? 플레이어 이동속도 설정
        Speed = 0.1f;
        _PlayerBoxColider2D = GetComponent<BoxCollider2D>();
        _PlayerAnimator = GetComponent<Animator>();

        //Debug.Log(_PlayerLayMask.value);

       // Observable.EveryFixedUpdate()
       //.Where(_ => Input.GetAxisRaw("Vertical") != 0 && Input.GetAxisRaw("Horizontal") !=0)
       //.Subscribe(_ =>
       //{
       //    vertical = Input.GetAxisRaw("Vertical");
       //    horizontal = Input.GetAxisRaw("Horizontal");

       //    if (is_canMove)
       //    {
       //        if (horizontal != 0 || vertical != 0)
       //        {
       //            is_canMove = false;
       //            StartCoroutine(ComputerMoveCoroutine());
       //        }
       //    }
       //});
    }
    
    private void Update()
    {
        if(!is_Joystick)    // 키보드
        {
            vertical = Input.GetAxisRaw("Vertical");
            horizontal = Input.GetAxisRaw("Horizontal");
        }
        else  // 조이스틱
        {
            vertical = joystick.GetVerticalValue();
            horizontal = joystick.GetHorizontalValue();
        }     
         if (is_canMove && DialogueManager.is_keypad && !DialogueManager.is_Msg)
        {
            if(horizontal != 0 || vertical != 0)
            {
                is_canMove = false;
                StartCoroutine(ComputerMoveCoroutine());
            }
        }
    }
    
    IEnumerator ComputerMoveCoroutine()
    {
        while(vertical != 0 || horizontal != 0)
        {            
            _PlayerVector.Set(horizontal, vertical);
            _PlayerVector.Normalize();
            //Debug.Log(_PlayerVector);
            //if (_PlayerVector.x != 0) _PlayerVector.y = 0;
            _PlayerAnimator.SetFloat("DirX", _PlayerVector.x);
            _PlayerAnimator.SetFloat("DirY", _PlayerVector.y);
            _PlayerAnimator.SetBool("Walking", true);

            //? 이동 불가 지역 설정
            RaycastHit2D hit;
            Vector2 _start = transform.position;
            Vector2 _end = _start + new Vector2(_PlayerVector.x * speed * walkCount, _PlayerVector.y * speed *walkCount);

            _PlayerBoxColider2D.enabled = false;
            hit = Physics2D.Linecast(_start, _end, _PlayerLayMask);
            _PlayerBoxColider2D.enabled = true;

            if(hit.transform != null)
            {               
                break;
            }
            //Debug.Log(hit.transform);
            while(currentWalkCount < walkCount)
            {
                transform.Translate(_PlayerVector.x * (speed + runspeed), _PlayerVector.y * (speed + runspeed), 0);
               // if (is_applyRunFlag) currentWalkCount++;
                currentWalkCount++;
                yield return new WaitForSeconds(0.01f);
            }
            currentWalkCount = 0;
                        
        }
        _PlayerAnimator.SetBool("Walking", false);
        is_canMove = true;
            }

3.캐릭터와 상호작용 및 선택답안1

![image](https://user-images.githubusercontent.com/48191157/71571323-108ddd00-2b1d-11ea-9f29-8f8f03256b08.png)

3_1.캐릭터와 상호작용 및 선택답안2

![image](https://user-images.githubusercontent.com/48191157/71571329-184d8180-2b1d-11ea-88c7-c22b7e69c441.png)

4.스탯에 따른 다른 스토리 및 결말

![image](https://user-images.githubusercontent.com/48191157/71571337-1edbf900-2b1d-11ea-8f4f-a35c1ab0c9be.png)
