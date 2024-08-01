---
---
#ifdef \_APPLE\_
#include <GLUT/glut.h>
#else
#include <GL/glut.h>
#endif

#include <stdlib.h>
#include <valarray>
#include <iostream>
#include <bits/stdc++.h>
using namespace std;
bool matchColor(int x, int y, int r, int g, int b)
{

    cout<<x<<" "<<y<<endl;
    // \[x,y,z\] pixel color values will be stored in the pixel array
    float pixel\[4\];

    glReadPixels(x, y, 1, 1, GL\_RGB, GL\_FLOAT, pixel);
cout << "R: " << pixel\[0\] << endl;
    cout << "G: " << pixel\[1\] << endl;
    cout << "B: " << pixel\[2\] << endl;

    if (pixel\[0\] == r && pixel\[1\] == g && pixel\[2\] == b)
        return true;
    return false;
}
void BoundaryFill4(int x,int y,float fillR,float fillG,float fillB,float boundaryR,float boundaryG,float boundaryB)
{
  //  if(matchColor(x,y,boundaryR,boundaryG,boundaryB))

    float color\[3\];
    glReadPixels(x,y,1.0,1.0,GL\_RGB,GL\_FLOAT,color);
    if((color\[0\]!=boundaryR|| color\[1\]!=boundaryG || color\[2\]!=boundaryB)&&(
    color\[0\]!=fillR || color\[1\]!=fillG|| color\[2\]!=fillB)){
        glColor3f(fillR,fillG,fillB);
        glBegin(GL\_POINTS);
        glVertex2i(x,y);
        glEnd();
        glFlush();
        BoundaryFill4(x+1,y,fillR,fillG,fillB,boundaryR,boundaryG,boundaryB);
        BoundaryFill4(x-1,y,fillR,fillG,fillB,boundaryR,boundaryG,boundaryB);
        BoundaryFill4(x,y+1,fillR,fillG,fillB,boundaryR,boundaryG,boundaryB);
        BoundaryFill4(x,y-1,fillR,fillG,fillB,boundaryR,boundaryG,boundaryB);
    }
}
void floodFill4(int x, int y, float fillColorRed,
                float fillColorGreen, float fillColorBlue)
{
    // get color of seed pixel
    float color\[3\];
    glReadPixels(x, y, 1, 1, GL\_RGB, GL\_FLOAT, color);

    // seed pixel is already the desired color
    if (color\[0\] == 0 && color\[1\] == 0 && color\[2\] == 0)

    // seed pixel is not the desired color
    // so replace the color
  { glColor3f(fillColorRed, fillColorGreen, fillColorBlue);
    glBegin(GL\_POINTS);
    glVertex2i(x, y);
    glEnd();
    glFlush();

    // recursively call floodFill4 on neighboring pixels
    floodFill4(x + 1, y, fillColorRed, fillColorGreen, fillColorBlue);
    floodFill4(x - 1, y, fillColorRed, fillColorGreen, fillColorBlue);
    floodFill4(x, y + 1, fillColorRed, fillColorGreen, fillColorBlue);
    floodFill4(x, y - 1, fillColorRed, fillColorGreen, fillColorBlue);

  }
}
static void display(void)
{

    glClear(GL\_COLOR\_BUFFER\_BIT);
    glColor3f(0.5,0.5,0.5);
    glBegin(GL\_POINTS);
    glVertex3f(0.0,0.0,0.0);

    glEnd();
    glColor3f(1,0,0);
    glBegin(GL\_LINES);
    glVertex2i(10,30);
    glVertex2i(50,10);
    glVertex2i(50,10);

    glVertex2i(70,25);
    glVertex2i(70,25);

    glVertex2i(60,35);
    glVertex2i(60,35);

    glVertex2i(60,60);
    glVertex2i(60,60);

    glVertex2i(10,30);

//  // rectangle of 100\*100
    glVertex2i(100,100);
    glVertex2i(200,100);
    glVertex2i(200,100);
    glVertex2i(200,200);
    glVertex2i(200,200);
    glVertex2i(100,200);
    glVertex2i(100,200);
    glVertex2i(100,100);

    glEnd();
    glFlush();
    BoundaryFill4(20,30,1,0,1,1,0,0);
    floodFill4(120,130,1,1,1);

}

// floodfill 4 connected with
// seed point (x,y) and
// replacement color defined
// by (fillColorRed, fillColorGreen, fillColorBlue)

void init();

int main(int argc, char \*argv\[\])
{
    glutInit(&argc, argv);
    glutInitWindowSize(700,700);
    glutInitDisplayMode(GLUT\_RGB );

    glutCreateWindow("Flood Fill");
    init();
    glutDisplayFunc(display);
    glutMainLoop();

    return EXIT\_SUCCESS;
}

void init() {
    glClearColor(0,0,0,0.0);
    glOrtho(0,700,0,700,-10.0,10.0);
    glMatrixMode(GL\_PROJECTION);
}
