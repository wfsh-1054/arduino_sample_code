#include "Emakefun_MotorDriver.h" //馬達驅動
#include "PS2X_lib.h"  //PS2 遙控

//定義遙控接腳 
#define PS2_DAT     12
#define PS2_CMD     11
#define PS2_SEL     10
#define PS2_CLK     13

//#define pressures   true
#define pressures   false
//#define rumble      true
#define rumble      false

//控制板初始化函數
PS2X ps2x; // create PS2 Controller Class
Emakefun_MotorDriver mMotor = Emakefun_MotorDriver(0x60);
Emakefun_DCMotor *LeftFoward, *RightFoward, *LeftBackward, *RightBackward;

int Speed;

//馬達初始化設定
void MotorInit(int leftward, int rightfoward, int leftbackward, int rightbackward)
{
    LeftFoward = mMotor.getMotor(leftward);
    RightFoward = mMotor.getMotor(rightfoward);
    LeftBackward = mMotor.getMotor(leftbackward);
    RightBackward = mMotor.getMotor(rightbackward);
    delay(200);
    mMotor.begin(50);
}

//轉速定義函數
void SetSpeed(int8_t s)
{
    if (s > 100) {
        Speed = 100;
        return;
    } else if (s < 0) {
        Speed = 0;
        return;
    }
    Speed = s;
}

//偵測速度數值回傳
int GetSpeed(void)
{
    return Speed;
}

//加速
void SpeedUp(int8_t Duration)
{
    SetSpeed(Speed + Duration);
}

//減速
void SpeedDown(int8_t Duration )
{
    SetSpeed(Speed - Duration);
}

設定轉速
void DriveSpeed(int s)
{
  if (s >= 0 && s <= 100) {
    LeftFoward->setSpeed((s / 10) * 25.5);
    RightFoward->setSpeed((s / 10) * 25.5);
    LeftBackward->setSpeed((s / 10) * 25.5);
    RightBackward->setSpeed((s / 10) * 25.5);
  }
}

//前進
void GoForward(void)
{
  DriveSpeed(Speed);
  LeftFoward->run(FORWARD);
  RightFoward->run(FORWARD);
  LeftBackward->run(FORWARD);
  RightBackward->run(FORWARD);
}

//後退
void GoBack(void)
{
  DriveSpeed(Speed);
  LeftFoward->run(BACKWARD);
  RightFoward->run(BACKWARD);
  LeftBackward->run(BACKWARD);
  RightBackward->run(BACKWARD);
}

//停止
void KeepStop(void)
{
  DriveSpeed(0);
  LeftFoward->run(BRAKE);
  RightFoward->run(BRAKE);
  LeftBackward->run(BRAKE);
  RightBackward->run(BRAKE);
}

//向左前進
void TurnLeft(void)
{
  int s;
  s = (Speed / 10) * 25.5;
  LeftFoward->setSpeed(s/2);
  LeftBackward->setSpeed(s/2);
  RightFoward->setSpeed(s);
  RightBackward->setSpeed(s);
  LeftFoward->run(FORWARD);
  RightFoward->run(FORWARD);
  LeftBackward->run(FORWARD);
  RightBackward->run(FORWARD);
}

//向右前進
void TurnRight(void)
{
    int s;
    s = (Speed / 10) * 25.5;
    LeftFoward->setSpeed(s);
    LeftBackward->setSpeed(s);
    RightFoward->setSpeed(s/2);
    RightBackward->setSpeed(s/2);
    LeftFoward->run(FORWARD);
    RightFoward->run(FORWARD);
    LeftBackward->run(FORWARD);
    RightBackward->run(FORWARD);
}

//向左轉
void TurnLeftRotate(void)
{
    DriveSpeed(Speed);
    LeftFoward->run(BACKWARD);
    LeftBackward->run(BACKWARD);
    RightFoward->run(FORWARD);
    RightBackward->run(FORWARD);
}

//向右轉
void TurnRightRotate(void)
{
    DriveSpeed(Speed);
    LeftFoward->run(FORWARD);
    LeftBackward->run(FORWARD);
    RightFoward->run(BACKWARD);
    RightBackward->run(BACKWARD);
}

void setup()
{
  Serial.begin(9600);
  //定義馬達位置
  MotorInit(M2, M1, M4, M3);
  //setup pins and settings: GamePad(clock, command, attention, data, Pressures?, Rumble?) check for error
  //設定搖桿連線
  ps2x.config_gamepad(PS2_CLK, PS2_CMD, PS2_SEL, PS2_DAT, pressures, rumble);

}

void loop()
{
  //搖桿讀取
  ps2x.read_gamepad();
  if (ps2x.ButtonDataByte()) {
    //加速
    if (ps2x.Button(PSB_TRIANGLE)) {
      Serial.print("PS2X PSB_TRIANGLE:");
      SpeedUp(5);
      Serial.println(GetSpeed());
      return;
    }
    //減速
    if (ps2x.Button(PSB_CROSS)) {
      Serial.print("PS2X PSB_CROSS:");
      SpeedDown(5);
      Serial.println(GetSpeed());
      return;
    }
    //前進
    if (ps2x.Button(PSB_PAD_UP)) {
      Serial.println("PS2X PSB_PAD_UP:");
      GoForward();
    }
    //後退
    if (ps2x.Button(PSB_PAD_DOWN)) {
      Serial.println("PS2X PSB_PAD_DOWN:");
      GoBack();
    }
    //向左前進
    if (ps2x.Button(PSB_PAD_LEFT)) {
      Serial.println("PS2X PSB_PAD_LEFT:");
      TurnLeft();
    }
    //向右進
    if (ps2x.Button(PSB_PAD_RIGHT)) {
      Serial.println("PS2X PSB_PAD_RIGHT:");
      TurnRight();
    }
    //向左轉
    if (ps2x.Button(PSB_SQUARE)) {
      Serial.println("PS2X PSB_SQUARE:");
      TurnLeftRotate();
    }
    //向右轉
    if (ps2x.Button(PSB_CIRCLE)) {
      Serial.println("PS2X PSB_CIRCLE:");
      TurnRightRotate();
    }
  } else {
    //停止
    KeepStop();
  }
}
