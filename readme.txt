#pragma comment(linker, "/SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup")
#pragma comment(lib,"xinput.lib ")
#include <windows.h>
#include <XInput.h>
#include <GL/freeglut/freeglut.h>
#define WIDTH 320
#define HEIGHT 240
int x=150,y=80;
#define MAX_CONTROLLERS  4  //XInput���F���ł���̂�4�܂�
#define Threshold   65535/4 //�������l
struct CONTROLER_STATE
{
    XINPUT_STATE state;
    bool bConnected;
};
CONTROLER_STATE GAME_PAD[MAX_CONTROLLERS];
//�ڑ�����Ă��邩���ׂ�
HRESULT UpdateControllerState()
{
    DWORD dwResult;
    for( DWORD i = 0; i < MAX_CONTROLLERS; i++ )
    {
        dwResult = XInputGetState( i, &GAME_PAD[i].state );
        if( dwResult == ERROR_SUCCESS )
            GAME_PAD[i].bConnected = true;
        else
            GAME_PAD[i].bConnected = false;
    }
    return S_OK;
}
void SquareFill2D(int x1,int y1,int x2, int y2){
 glBegin(GL_QUADS);
 glVertex2i(x1,y1);
 glVertex2i(x2,y1);
 glVertex2i(x2,y2);
 glVertex2i(x1,y2);
 glEnd();
}
void display(void)
{
 glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
 glColor4f(0.0f,0.5f,0.0f,1.0f);
 SquareFill2D(x,y,x+50,y+50);
 glutSwapBuffers();
}
void idle(void)
{
 UpdateControllerState();
 
    for( DWORD i = 0; i < MAX_CONTROLLERS; i++ ){
        if( GAME_PAD[i].bConnected ){
            WORD Buttons = GAME_PAD[i].state.Gamepad.wButtons;
   //�A�i���O�����L�[
   if(GAME_PAD[i].state.Gamepad.sThumbLY>Threshold)y-=2;
   if(GAME_PAD[i].state.Gamepad.sThumbLY<-Threshold)y+=2;
   if(GAME_PAD[i].state.Gamepad.sThumbLX>Threshold)x+=2;
   if(GAME_PAD[i].state.Gamepad.sThumbLX<-Threshold)x-=2;
   //�f�W�^�������L�[
   if( Buttons & XINPUT_GAMEPAD_DPAD_UP )y-=2;
   if( Buttons & XINPUT_GAMEPAD_DPAD_DOWN )y+=2;
   if( Buttons & XINPUT_GAMEPAD_DPAD_LEFT )x-=2;
   if( Buttons & XINPUT_GAMEPAD_DPAD_RIGHT )x+=2;
   //�e��{�^�� �󐧌� �R�s�y�p
   if( Buttons & XINPUT_GAMEPAD_A );
   if( Buttons & XINPUT_GAMEPAD_B );
   if( Buttons & XINPUT_GAMEPAD_X );
   if( Buttons & XINPUT_GAMEPAD_Y );
   if( Buttons & XINPUT_GAMEPAD_START );
   if( Buttons & XINPUT_GAMEPAD_BACK );
   if( Buttons & XINPUT_GAMEPAD_LEFT_THUMB );//���A�i���O�����L�[���p�b�h���ɉ������ꍇ
   if( Buttons & XINPUT_GAMEPAD_RIGHT_THUMB );//�E�A�i���O�����L�[���p�b�h���ɉ������ꍇ
   if( Buttons & XINPUT_GAMEPAD_LEFT_SHOULDER );
   if( Buttons & XINPUT_GAMEPAD_RIGHT_SHOULDER );
   if(GAME_PAD[i].state.Gamepad.bLeftTrigger);
   if(GAME_PAD[i].state.Gamepad.bRightTrigger);
        }else{
            //�ڑ�����Ă��Ȃ��ꍇ
        }
 }
 Sleep(1);
 glutPostRedisplay();
}
void Init(){
 glClearColor(1.0, 1.0, 1.0, 1.0);
 glOrtho(0, WIDTH, HEIGHT, 0, -1, 1);
}
int main(int argc, char *argv[])
{
 glutInitWindowPosition(100, 100);
 glutInitWindowSize(WIDTH, HEIGHT);
 glutInit(&argc, argv);
 glutInitDisplayMode(GLUT_RGBA);
 glutCreateWindow("�W���C�p�b�h");
 glutDisplayFunc(display);
 glutIdleFunc(idle);
 Init();
 glutMainLoop();
 return 0;
}


