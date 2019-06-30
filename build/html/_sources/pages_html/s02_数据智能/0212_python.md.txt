## cv


version1
```python
# cv1 version
import cv2
import time
import numpy as np
import curses

# pixel后面4位都是空格，代表像素值比较大的
# 近似白色背景
pixels = " .,-'`:!1+*%    "


def get_images(video_name, size):
    '''
    视频帧分解
    图像转灰度图，再缩放
    :param video_name:
    :param size:
    :return: 灰度图列表
    '''
    img_list = []
    cap = cv2.VideoCapture(video_name)
    while cap.isOpened():
        ret, frame = cap.read()
        if ret:
            gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
            img = cv2.resize(gray, size, interpolation=cv2.INTER_AREA)
            img_list.append(img)
        else:
            break
    cap.release()
    return img_list


def img_2_char(img):
    '''
    灰度图像映射成字符[0,255] -> [0,16]
    用[0,16]索引值取字符
    :param img:
    :return:  字符数组
    '''
    ret = []
    p = img / 255
    indexes = (p * (len(pixels) - 1)).astype(np.int)
    height, width = img.shape
    for row in range(height):
        line = ""
        for col in range(width):
            index = indexes[row][col]
            line += pixels[index] + " "
        ret.append(line)
    return ret


def video_2_char(img_list):
    '''
    图像列表 转换成 字符图列表
    :param img_list:
    :return:
    '''
    video_char = []
    for img in img_list:
        pic = img_2_char(img)
        video_char.append(pic)

    return video_char


def play_video(video_char):
    width, height = len(video_char[0][0]), len(video_char[0])
    stdscr = curses.initscr()
    curses.start_color()
    try:
        stdscr.resize(height, width * 2)
        for pic_i in range(len(video_char)):
            for line_i in range(height):
                # 将pic_i的第i行写入第i列。(line_i, 0)表示从第i行的开头开始写入。最后一个参数设置字符为白色
                stdscr.addstr(line_i, 0, video_char[pic_i][line_i], curses.COLOR_WHITE)
            stdscr.refresh()  # 写入后需要refresh才会立即更新界面
            time.sleep(1 / 24)
    finally:
        curses.endwin()


if __name__ == "__main__":
    imgs = get_images('c:\\video\\kun.mp4', (64, 40))
    video_chars = video_2_char(imgs)
    play_video(video_chars)
```

version2
```python
# cv version2
import cv2
import time
import numpy as np
import curses

# pixel后面4位都是空格，代表像素值比较大的
# 近似白色背景
pixels = " .,-'`:!1+*%    "


def get_images(video_name, size):
    '''
    视频帧分解
    图像转灰度图，再缩放
    :param video_name:
    :param size:
    :return: 灰度图列表
    '''
    img_list = []
    cap = cv2.VideoCapture(video_name)
    while cap.isOpened():
        ret, frame = cap.read()
        if ret:
            gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
            img = cv2.resize(gray, size, interpolation=cv2.INTER_AREA)
            img_list.append(img)
        else:
            break
    cap.release()
    return img_list


def img_2_char(img):
    '''
    灰度图像映射成字符[0,255] -> [0,16]
    用[0,16]索引值取字符
    :param img:
    :return:  字符数组
    '''
    ret = []
    p = img / 255
    indexes = (p * (len(pixels) - 1)).astype(np.int)
    height, width = img.shape
    for row in range(height):
        line = ""
        for col in range(width):
            index = indexes[row][col]
            line += pixels[index] + " "
        ret.append(line)
    return ret


def video_2_char(img_list):
    '''
    图像列表 转换成 字符图列表
    :param img_list:
    :return:
    '''
    video_char = []
    for img in img_list:
        pic = img_2_char(img)
        video_char.append(pic)

    return video_char


def play_video(video_char):
    width, height = len(video_char[0][0]), len(video_char[0])
    stdscr = curses.initscr()
    curses.start_color()
    try:
        stdscr.resize(height, width * 2)
        for pic_i in range(len(video_char)):
            for line_i in range(height):
                # 将pic_i的第i行写入第i列。(line_i, 0)表示从第i行的开头开始写入。最后一个参数设置字符为白色
                stdscr.addstr(line_i, 0, video_char[pic_i][line_i], curses.COLOR_WHITE)
            stdscr.refresh()  # 写入后需要refresh才会立即更新界面
            time.sleep(1 / 24)
    finally:
        curses.endwin()


if __name__ == "__main__":
    imgs = get_images('c:\\video\\kun.mp4', (64, 40))
    video_chars = video_2_char(imgs)
    play_video(video_chars)
```

## 归约
归约是使用解题的"黑盒"来解决另一个问题的思维方式。归约让我们理解两个问题间的关系，它是一种技术，用于寻找解决某个问题或它的变形。
我们解题时常遇见似曾相识的题目。此时，我们若可将新题转换成已解旧题的一例，则新题亦解矣。
另一更微妙的用法是：若我们拥有一个已证明难以解决的问题，我们又获得另一个相似的新问题。我们可合理推想此新问题亦是难以解决的。我们可由下列谬证法得证：若此新问题本质上容易解答，且若我们可展示每个旧问题的实例可经由一系列转换步骤变成新问题的实例，则旧问题便容易解决，因此得到悖论。因此新问题可知亦难以解决。
一个归约简例是从乘法化成平方。设想我们仅能以加、减、平方与除以二等操作，我们可运用此知识并结合下列方程式，以取得任两数的乘积：
a×b= ((a + b)- a- b)/2.
我们亦可从另一方向归约此问题：显然地，若我们可以乘以任两数，则我们可以对任一数平方：
a=a×a.
因此可见两问题之难度似乎相等，此类归约称为图灵归约。
然而，若我们增加条件：“此运算只能使用平方一次，且只能在结尾使用”，则更难寻找合适归约。在这条件下，即使我们使用所有基础运算，包括乘法，也找不到适当的归约步骤。因为我们不仅要运算有理数，也必须运算像是根号2的无理数。而另一方向的归约，我们的确可用一次乘法简单地平方任何数，且此乘法的确是最后的运算。将此限制形式导入归约中，我们已展示其归约结论：普遍来说，乘法的确难于平方。此归约称为多一归约 [1]  。





## 3.python操作sqlalchemy

### 3.1需要的包

```bash
pip install sqlalchemy psycopg2
```

### 3.2mac安装

```bash
pip install psycopg2-binary # 这里psy直接装是有问题的，可以这样装
```



### 3.3准备工作

- 文档

> https://www.programcreek.com/python/example/82340/sqlalchemy.dialects.postgresql.JSONB



- python postgres 实例

> https://blog.csdn.net/Bear_861110453/article/details/83412985

- 官方文档

>https://docs.sqlalchemy.org/en/13/dialects/postgresql.html#sqlalchemy.dialects.postgresql.JSONB

## 4.python处理pandas

