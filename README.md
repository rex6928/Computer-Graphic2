# Computer-Graphic2
컴퓨터 그래픽스 과제 제출용 리포지토리

#include <GL/glut.h>

float angle = 0.0f; // 회전 각도

void init() {
    glClearColor(0.0, 0.0, 0.0, 1.0); // 배경색 설정: 검정색
    glEnable(GL_DEPTH_TEST); // 깊이 테스트 활성화
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT); // 컬러와 깊이 버퍼 초기화
    glLoadIdentity(); // 현재 변환 행렬 초기화

    glTranslatef(0.0f, 0.0f, -5.0f); // 주전자를 화면 중심에 위치시키기 위해 Z축으로 이동
    glRotatef(angle, 0.0f, 1.0f, 0.0f); // Y축을 기준으로 회전

    glutSolidTeapot(1.0); // 주전자 그리기

    glutSwapBuffers(); // 프레임 버퍼 교체
}

void reshape(int w, int h) {
    glViewport(0, 0, w, h); // 뷰포트 설정
    glMatrixMode(GL_PROJECTION); // 투영 행렬 설정
    glLoadIdentity();
    gluPerspective(45.0, (double)w / (double)h, 1.0, 200.0); // 원근 투영 설정
    glMatrixMode(GL_MODELVIEW); // 모델뷰 행렬로 변경
}

void timer(int value) {
    angle += 1.0f; // 회전 각도 증가
    if (angle > 360.0f) {
        angle -= 360.0f; // 360도를 넘어가면 0도로 되돌림
    }
    glutPostRedisplay(); // 디스플레이 콜백 재호출
    glutTimerFunc(16, timer, 0); // 16ms 후에 타이머 재설정 (약 60FPS)
}

int main(int argc, char** argv) {
    glutInit(&argc, argv); // GLUT 초기화
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH); // 디스플레이 모드 설정
    glutInitWindowSize(800, 600); // 윈도우 크기 설정
    glutCreateWindow("Rotating Teapot"); // 윈도우 생성 및 제목 설정

    init(); // 초기화 함수 호출

    glutDisplayFunc(display); // 디스플레이 콜백 설정
    glutReshapeFunc(reshape); // 리쉐이프 콜백 설정
    glutTimerFunc(0, timer, 0); // 타이머 콜백 설정

    glutMainLoop(); // 메인 루프 진입

    return 0;
}
