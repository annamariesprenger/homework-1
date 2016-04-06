# homework-1
homework 1
// This is the CPP file you will edit and turn in.
// Also remove these comments here and add your own.
// TODO: rewrite this comment

#include <iostream>
#include "console.h"
#include "gwindow.h"
#include "grid.h"
#include "simpio.h"
#include "strlib.h"
#include "gbufferedimage.h"
#include "gevents.h"
#include "math.h" //for sqrt and exp in the optional Gaussian kernel
using namespace std;

static const int    WHITE = 0xFFFFFF;
static const int    BLACK = 0x000000;
static const int    GREEN = 0x00FF00;
static const double PI    = 3.14159265;
static const int    SCATTER = 1;
static const int    EDGE_DETECT = 2;
static const int    GREEN_SCREEN = 3;
static const int    COMPARE = 4;

static void     doFauxtoshop(GWindow &gw, GBufferedImage &img);
static bool     openImageFromFilename(GBufferedImage& img, string filename);static void     getMouseClickLocation(int &row, int &col);
static bool 	saveImageToFilename(const GBufferedImage &img, string filename);
static void     getMouseClickLocation(int &row, int &col);
static void     askForFilter(GWindow &gw,int& filterChoice, GBufferedImage& img);
static void     applyScatter(GBufferedImage& img);
static void     applyEgdeDetect(GBufferedImage& img);
static void     applyGreenScreen(GBufferedImage& img);
static void     applyCompare(GWindow &gw,GBufferedImage& img, GBufferedImage& new_img);


///////////////////////////////////////////////////
/* STARTER CODE FUNCTION - DO NOT EDIT
 *
 * This main simply declares a GWindow and a GBufferedImage for use
 * throughout the program. By asking you not to edit this function,
 * we are enforcing that the GWindow have a lifespan that spans the
 * entire duration of execution (trying to have more than one GWindow,
 * and/or GWindow(s) that go in and out of scope, can cause program
 * crashes).
 */
int main() {
    GWindow gw;
    gw.setTitle("Fauxtoshop");
    gw.setVisible(true);
    GBufferedImage img;
    doFauxtoshop(gw, img);
    return 0;
}

/* This is yours to edit. Depending on how you approach your problem
 * decomposition, you will want to rewrite some of these lines, move
 * them inside loops, or move them inside helper functions, etc.
 *
 * TODO: rewrite this comment.
 */
static void doFauxtoshop(GWindow &gw, GBufferedImage &img) {
    cout << "Welcome to Fauxtoshop!" << endl;
    cout << "Enter the name of image file to open (or blank to quit): ";
    string imageName;
    cin >> imageName;
       while(openImageFromFilename(img,imageName)== false ){ //deal with shitty input
            cout << "Invalid file name." << endl;
            cout << "Welcome to Fauxtoshop!" << endl;
            cout << "Enter the name of image file to open (or blank to quit): ";
            cin >> imageName;
        }

       cout << "Opening image file, may take a minute...";
        openImageFromFilename(img,imageName);
    gw.setCanvasSize(img.getWidth(), img.getHeight());
    gw.add(&img,0,0);
    int row, col;
    int filter;//uh
    askForFilter(gw, filter, img);//yeah stop being trash

    getMouseClickLocation(row, col); //clicking after fliter has been applied clears it
    gw.clear();
}


void askForFilter(GWindow &gw, int& filterChoice, GBufferedImage& img){ //adding gw thing
  //initialize
    cout << "Which image filter would you like to apply?" << endl;
    cout << "\t" << "1-Scatter" <<endl;
    cout << "\t" << "2-Edge Detection" <<endl;
    cout << "\t" << "3-Green Screen with another image" <<endl;
    cout << "\t" << "4-Compare with another image" <<endl;
    cout << "Your choice: ";
    cin  >> filterChoice; //this can be corrected
        if (filterChoice == SCATTER){
         applyScatter(img);
}
        else if (filterChoice == EDGE_DETECT) {
               applyEgdeDetect(img);
}
                else if (filterChoice == GREEN_SCREEN) {
                 applyGreenScreen(img);
}
                else if (filterChoice == COMPARE) {
                GBufferedImage new_img;
               applyCompare(gw,img, new_img);
               cout << "this far!";
}
                else {
                //deal with the shitty input
            cout <<"look im garbage";
}
}

static void applyScatter(GBufferedImage& img){
    //code
    cout << "scatter";
}
static void applyEgdeDetect(GBufferedImage &img){
    //code
    cout <<"edge";
}

static void applyGreenScreen(GBufferedImage &img){
    //code
    cout << "green";
}

static void applyCompare(GWindow &gw, GBufferedImage& img, GBufferedImage& new_img){
    cout << "Now choose another image file to compare to." << endl;
    cout << "Enter name of image file to open: ";
    string imageName;
    cin >> imageName; //taking input for new image
    while(openImageFromFilename(new_img,imageName)== false ){ //deal with shitty input
         cout << "Invalid file name." << endl;
         cout << "Now choose another image file to compare to." << endl;
         cout << "Enter name of image file to open: ";
         cin >> imageName;
     }
    cout << endl <<"Opening image file, may take a minute...";

    openImageFromFilename(new_img,imageName);
gw.setCanvasSize(img.getWidth(), img.getHeight());
gw.add(&img,0,0);
int row, col;
cout << "yay you made it!";
}


//////////////dont edit past here////////////////////////////////////////////////////
/* STARTER CODE HELPER FUNCTION - DO NOT EDIT
 *
 * Attempts to open the image file 'filename'.
 *
 * This function returns true when the image file was successfully
 * opened and the 'img' object now contains that image, otherwise it
 * returns false.
 */
static bool openImageFromFilename(GBufferedImage& img, string filename) {
    try { img.load(filename); }
    catch (...) { return false; }
    return true;
}

/* STARTER CODE HELPER FUNCTION - DO NOT EDIT
 *
 * Attempts to save the image file to 'filename'.
 *
 * This function returns true when the image was successfully saved
 * to the file specified, otherwise it returns false.
 */
static bool saveImageToFilename(const GBufferedImage &img, string filename) {
    try { img.save(filename); }
    catch (...) { return false; }
    return true;
}

/* STARTER CODE HELPER FUNCTION - DO NOT EDIT
 *
 * Waits for a mouse click in the GWindow and reports click location.
 *
 * When this function returns, row and col are set to the row and
 * column where a mouse click was detected.
 */
static void getMouseClickLocation(int &row, int &col) {
    GMouseEvent me;
    do {
        me = getNextEvent(MOUSE_EVENT);
    } while (me.getEventType() != MOUSE_CLICKED);
    row = me.getY();
    col = me.getX();
}

/* OPTIONAL HELPER FUNCTION
 *
 * This is only here in in case you decide to impelment a Gaussian
 * blur as an OPTIONAL extension (see the suggested extensions part
 * of the spec handout).
 *
 * Takes a radius and computes a 1-dimensional Gaussian blur kernel
 * with that radius. The 1-dimensional kernel can be applied to a
 * 2-dimensional image in two separate passes: first pass goes over
 * each row and does the horizontal convolutions, second pass goes
 * over each column and does the vertical convolutions. This is more
 * efficient than creating a 2-dimensional kernel and applying it in
 * one convolution pass.
 *
 * This code is based on the C# code posted by Stack Overflow user
 * "Cecil has a name" at this link:
 * http://stackoverflow.com/questions/1696113/how-do-i-gaussian-blur-an-image-without-using-any-in-built-gaussian-functions
 *
 */
static Vector<double> gaussKernelForRadius(int radius) {
    if (radius < 1) {
        Vector<double> empty;
        return empty;
    }
    Vector<double> kernel(radius * 2 + 1);
    double magic1 = 1.0 / (2.0 * radius * radius);
    double magic2 = 1.0 / (sqrt(2.0 * PI) * radius);
    int r = -radius;
    double div = 0.0;
    for (int i = 0; i < kernel.size(); i++) {
        double x = r * r;
        kernel[i] = magic2 * exp(-x * magic1);
        r++;
        div += kernel[i];
    }
    for (int i = 0; i < kernel.size(); i++) {
        kernel[i] /= div;
    }
    return kernel;
}
