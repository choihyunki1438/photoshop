# photoshop
# photoshop program

# 먼저 사용할 모듈을 가져온다.
import sys
import cv2
from PySide6.QtGui import (QAction, QImage, QPixmap)
from PySide6.QtWidgets import (
    QApplication, QWidget, QLabel, QMainWindow, QToolTip, 
    QHBoxLayout, QVBoxLayout, QPushButton, QFileDialog
)


class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.initUI()
    

        # 메인메뉴바 생성
        self.menu = self.menuBar()
        self.menu_file = self.menu.addMenu("파일")
        exit = QAction("나가기", self, triggered=qApp.quit)
        self.menu_file.addAction(exit)
        
        # 회전 메뉴바 생성
        self.menu_file = self.menu.addMenu("회전")
        # 회전 메뉴를 넣어준다.
        img90 = QAction("90도", self, triggered=self.img90_image) ##def 함수로 지정한함수명
        self.menu_file.addAction(img90)
        img180 = QAction("180도", self, triggered=self.img180_image)
        self.menu_file.addAction(img180)
        img270 = QAction("270도", self, triggered=self.img270_image)
        self.menu_file.addAction(img270)

        # 필터 효과 메뉴바 
        self.menu_file = self.menu.addMenu("필터 효과")
        # 필터 효과 메뉴를 넣어준다.
        blur = QAction("블러처리", self, triggered=self.blur_image)
        self.menu_file.addAction(blur)
        medianBlur = QAction("미디언 블러링", self, triggered=self.medianBlur_image)
        self.menu_file.addAction(medianBlur)
        bilateralFilter = QAction("바이레터널 필터", self, triggered=self.bilateralFilter_image)
        self.menu_file.addAction(bilateralFilter)
        cartoon = QAction("만화 필터", self, triggered=self.cartoon_image)
        self.menu_file.addAction(cartoon)
        detailEnhance = QAction("디테일 필터", self, triggered=self.detailEnhance_image)
        self.menu_file.addAction(detailEnhance)
        bitwise = QAction("비트와이즈", self, triggered=self.bitwise_image)
        self.menu_file.addAction(bitwise)


        # 메인화면 레이아웃
        main_layout = QHBoxLayout()

        # 사이드바 메뉴버튼
        sidebar = QVBoxLayout()
        button1 = QPushButton("이미지 열기")
        button2 = QPushButton("좌우반전")
        button3 = QPushButton("새로고침")      
        button1.clicked.connect(self.show_file_dialog)
        button2.clicked.connect(self.flip_image)
        button3.clicked.connect(self.clear_label)     

        sidebar.addWidget(button1)
        sidebar.addWidget(button2)
        sidebar.addWidget(button3)

        main_layout.addLayout(sidebar)

        #사진을 불러오기 위한 라벨 생성(왼쪽라벨에 넣어줄 사진원본)
        self.label1 = QLabel(self)
        self.label1.setFixedSize(650, 650)
        main_layout.addWidget(self.label1)

        #편집한 사진을 보기 위한 라벨(오른쪽 라벨에 넣어줄 편집사진)
        self.label2 = QLabel(self)
        self.label2.setFixedSize(650, 650)
        main_layout.addWidget(self.label2)       


        widget = QWidget(self)
        widget.setLayout(main_layout)
        self.setCentralWidget(widget)
    
    # 이미지를 편집할 함수선언
    # 포토샵 프로그램 종료함수
    def initUI(self):
      exitAction = QAction('Exit1', self)
      exitAction.setShortcut('Ctrl+Q') # 단축키로 종료 가능하도록 지정
      exitAction.setStatusTip('Exit application')
      exitAction.triggered.connect(qApp.quit)

      self.statusBar()

      self.toolbar = self.addToolBar('Exit')
      self.toolbar.addAction(exitAction)

      self.setWindowTitle('vision photoshop') # 프로그램명 지정
      self.setGeometry(300, 300, 300, 200)
      self.show()
    
    
    def show_file_dialog(self):
        file_name = QFileDialog.getOpenFileName(self, "이미지 열기", "./")
        self.image = cv2.imread(file_name[0])
        h, w, _ = self.image.shape
        bytes_per_line = 3 * w
        image = QImage(
            self.image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label1.setPixmap(pixmap)
        
        # 사진 반전할 수 있는 함수
    def flip_image(self):
        image = cv2.flip(self.image, 1)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap) #라벨 2에 편집한 이미지를 볼 수 있도록 선언


        #회전메뉴 함수 선언
    def img90_image(self):
        image = cv2.rotate(self.image, cv2.ROTATE_90_CLOCKWISE) # 시계방향으로 90도 회전 가능하도록 선언
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap)    

    def img180_image(self):
        image = cv2.rotate(self.image, cv2.ROTATE_180) # 180도 회전 가능하도록 선언
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap)  

    def img270_image(self):
        image = cv2.rotate(self.image, cv2.ROTATE_90_COUNTERCLOCKWISE) # 반시계 방향으로 90도(270도) 회전 가능하도록 선언
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap)  

        ## 필터메뉴 함수 선언
        #filter2D 함수를 이용해 블러처리하는 함수
    def add_image(self):
        image = cv2.filter2D(self.image, BLUR)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap) 

        # blur 처리할 수 있는 필터
    def blur_image(self):
        image = cv2.blur(self.image, (5, 5))
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap)  
        
        # 중앙값으로 대상 픽셀값을 대체하는 필터
    def medianBlur_image(self):
        image = cv2.medianBlur(self.image, 5)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap) 

        # bilateralFilte을 이용한 잡음제거 필터
    def bilateralFilter_image(self):
        image = cv2.bilateralFilter(self.image, 5, 75, 75)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap) 

        # stylization함수를 이용한 만화필터
    def cartoon_image(self):
        image = cv2.stylization(self.image, sigma_s=150, sigma_r=0.25)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap) 
        
        # 세부묘사를 이용해 강한 대비 효과를 주는 필터
    def detailEnhance_image(self):
        image = cv2.detailEnhance(self.image, sigma_s=10, sigma_r=0.15)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap)

        # 이미지를 합치거나 겹치는 등의 연산을 할 수 있는 비트와이즈 필터
    def bitwise_image(self):
        image = cv2.bitwise_not(self.image, 1)
        h, w, _ = image.shape
        bytes_per_line = 3 * w
        image = QImage(
            image.data, w, h, bytes_per_line, QImage.Format_RGB888
        ).rgbSwapped()
        pixmap = QPixmap(image)
        self.label2.setPixmap(pixmap)

    def clear_label(self):
        self.label2.clear()




if __name__ == "__main__":
    app = QApplication()
    window = MainWindow()
    window.show()
    sys.exit(app.exec())
