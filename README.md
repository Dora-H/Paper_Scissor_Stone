# Paper_Scissor_Stone
與電腦隨機出拳的三戰兩勝剪刀石頭布遊戲，搭配圖片顯示雙方各出了什麼拳。


## Strategy
1. 三戰兩勝制
2. 平手不出現圖示呈現
3. 遊戲開始可輸入玩家名稱
4. 以輸入數字代替0拳頭、1剪刀、2布
5. 與電腦隨機出拳方式比賽
6. 遊戲結束最後顯示幾勝幾負


## Requirements
● Python 3    
● random  
● time  
● string  
● cv2  
● numpy


## Class
● PaperScissorStone  


## Functions
● show  
● play  
● start 


## 建立基本屬性
#### 所有的標點符號＋空白（避免玩家輸入標點符號或是空白鍵非有效名稱）
    def __init__(self):
        self.all_charts = string.punctuation + string.whitespace
        
        
#### 依序為 : 石頭剪刀布選擇、回合初始化(1)、輸贏初始化(0)、圖示png列表
        self.loadings = ['rock', 'scissor', 'paper']
        self.choice = random.randint(0, 3)
        self.run = 1
        self.loser = 0
        self.winner = 0
        self.pngs = ['./rock.png', './scissor.png', './paper.png']


## 函數 
#### show 函數 : 呈現輸或是贏的雙方出拳結果圖示
    def show(self, you, computer, sign):
        img = ''
        if sign == 'win':
            if you == self.loadings[0] and computer == self.loadings[1]:
                img = np.hstack([cv2.imread(self.pngs[0]), cv2.imread(self.pngs[1])])
            elif you == self.loadings[1] and computer == self.loadings[2]:
                img = np.hstack([cv2.imread(self.pngs[1]), cv2.imread(self.pngs[2])])
            elif you == self.loadings[2] and computer == self.loadings[0]:
                img = np.hstack([cv2.imread(self.pngs[2]), cv2.imread(self.pngs[0])])
        elif sign == 'lose':
            if you == self.loadings[0] and computer == self.loadings[2]:
                img = np.hstack([cv2.imread(self.pngs[0]), cv2.imread(self.pngs[2])])
            elif you == self.loadings[1] and computer == self.loadings[0]:
                img = np.hstack([cv2.imread(self.pngs[1]), cv2.imread(self.pngs[0])])
            elif you == self.loadings[2] and computer == self.loadings[1]:
                img = np.hstack([cv2.imread(self.pngs[2]), cv2.imread(self.pngs[1])])
        cv2.imshow('You %s!' % sign, img)
        cv2.waitKey(0)
        
        
#### play 函數 : 遊戲開始
    def play(self, name):
        while self.run <= 3:
            try:
                choice = input('\nGuess(press q to quit) :\n 0)rock 1)scissor 2)paper: ')
                if choice.strip().lower() == 'q':
                    print('\033[31m See you next time, %s \033[0m' % name)
                    print('\033[31m Quit the game. \033[0m')
                    return
                if not choice:
                    continue

                computer = self.loadings[int(self.choice)]
                you = self.loadings[int(choice)]
                if int(choice)-self.choice == -1 or int(choice) - self.choice == 2:
                    print('%s:%s' % (name, you), '\ncomputer:', computer,
                          '\n\033[31mRun %d\033[31m' % self.run, '\033[31m %s win.\033[0m' % name)
                    self.winner += 1
                    self.show(you, computer, 'win')
                elif int(choice) == self.choice:
                    print('%s:%s' % (name, you), '\ncomputer:', computer,
                          '\n\033[31mRun %d\033[31m' % self.run, '\033[31m flat hand.\033[0m')
                else:
                    print('%s:%s' % (name, you), '\ncomputer:', computer,
                          '\n\033[31mRun %d\033[31m' % self.run, '\033[31m %s lose.\033[0m' % name)
                    self.loser += 1
                    self.show(you, computer, 'lose')
            finally:
                self.run += 1
        print('\033[31m Game Over.\033[0m 3 Runs, %s:%d win / %d lose.' % (name, self.winner, self.loser))


#### play 函數 : 遊戲啟動
    def start(self):
        while True:
#### 玩家可輸入自己名稱
            username = input('What is your name: ')
            for i in username:
#### 如果輸入內含任何標點符號、空白鍵，即為無效輸入，重新回上一層再輸入一次
                if i in self.all_charts:
                    print('Please enter valid name.')
                    continue
#### 如果輸入為有效，即顯示有效名稱
            if username.isalpha() is True:
                print('Valid name :%s.' % username)
                time.sleep(1)
                break
                ![valid](https://user-images.githubusercontent.com/70878758/129600374-1ec89df8-9450-452d-9f5b-91b39799d68c.png)

#### 倒數三秒，進入遊戲
        print("Are you ready? %s" % username)
        for i in range(3, -1, -1):
            print('+', "-"*4, "+")
            print("|", str(i).center(3), " |")
        print('+', "-"*4, "+")
        self.play(username)
        ![count](https://user-images.githubusercontent.com/70878758/129600275-c7388bf5-d59d-4c87-bbc0-487903ae696f.png)


####  主函數
    if __name__ == "__main__":
        player = PaperScissorStone()
        player.start()

