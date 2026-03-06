# 强化学习 Snake 算法

> 原文：<https://medium.com/mlearning-ai/reinforcement-learning-snake-algorithm-14a8d20dec7f?source=collection_archive---------3----------------------->

训练 snake 算法是与强化学习相关的最基本的项目之一。这有点像强化学习的“Hello World”。我最近发现了 python 工程师的 youtube 系列，其中他使用 pytorch 从头构建了一个强化学习算法，但我觉得他没有很好地解释概念或模型。我写这篇文章是为了解释算法的基本原理，也许还有它背后的一些数学知识。Python 工程师的 Youtube 视频:[https://youtu.be/PJl4iabBEz0](https://youtu.be/PJl4iabBEz0)。我们从简单地编写一个可以用 pygame 制作的贪吃蛇游戏开始

```
import pygameimport randomfrom enum import Enumfrom collections import namedtupleimport timepygame.init()font = pygame.font.Font('arial.ttf', 25)#font = pygame.font.SysFont('arial', 25)class Direction(Enum):RIGHT = 1LEFT = 2UP = 3DOWN = 4Point = namedtuple('Point', 'x, y')# rgb colorsWHITE = (255, 255, 255)RED = (200,0,0)BLUE1 = (0, 0, 255)BLUE2 = (0, 100, 255)BLACK = (0,0,0)BLOCK_SIZE = 20SPEED = 20class Snake:def __init__(self, w=640, h=480):self.w = wself.h = h# init displayself.display = pygame.display.set_mode((self.w, self.h))pygame.display.set_caption('Snake')self.clock = pygame.time.Clock()# init game stateself.direction = Direction.RIGHTself.head = Point(self.w/2, self.h/2)self.snake = [self.head,Point(self.head.x-BLOCK_SIZE, self.head.y),Point(self.head.x-(2*BLOCK_SIZE), self.head.y)]self.score = 0self.food = Noneself._place_food()def _place_food(self):x = random.randint(0, (self.w-BLOCK_SIZE )//BLOCK_SIZE )*BLOCK_SIZEy = random.randint(0, (self.h-BLOCK_SIZE )//BLOCK_SIZE )*BLOCK_SIZEself.food = Point(x, y)if self.food in self.snake:self._place_food()def play_step(self):# 1\. collect user inputfor event in pygame.event.get():if event.type == pygame.QUIT:pygame.quit()quit()if event.type == pygame.KEYDOWN:if event.key == pygame.K_LEFT:if self.direction == Direction.RIGHT:self.direction = Direction.RIGHTelse:self.direction = Direction.LEFTtime.sleep(0.03)elif event.key == pygame.K_RIGHT:if self.direction == Direction.LEFT:self.direction = Direction.LEFTelse:self.direction = Direction.RIGHTtime.sleep(0.03)elif event.key == pygame.K_UP:if self.direction == Direction.DOWN:self.direction = Direction.DOWNelse:self.direction = Direction.UPtime.sleep(0.03)elif event.key == pygame.K_DOWN:if self.direction == Direction.UP:self.direction = Direction.UPelse:self.direction = Direction.DOWNtime.sleep(0.03)# 2\. moveself._move(self.direction) # update the headself.snake.insert(0, self.head)# 3\. check if game overgame_over = Falseif self._is_collision():game_over = Truereturn game_over, self.score# 4\. place new food or just moveif self.head == self.food:self.score += 1self._place_food()else:self.snake.pop()# 5\. update ui and clockself._update_ui()self.clock.tick(SPEED)# 6\. return game over and scorereturn game_over, self.scoredef _is_collision(self):# hits boundaryif self.head.x > self.w - BLOCK_SIZE or self.head.x < 0 or self.head.y > self.h - BLOCK_SIZE or self.head.y < 0:return True# hits itselfif self.head in self.snake[1:]:return Truereturn Falsedef _update_ui(self):self.display.fill(BLACK)for pt in self.snake:pygame.draw.rect(self.display, BLUE1, pygame.Rect(pt.x, pt.y, BLOCK_SIZE, BLOCK_SIZE))pygame.draw.rect(self.display, BLUE2, pygame.Rect(pt.x+4, pt.y+4, 12, 12))pygame.draw.rect(self.display, RED, pygame.Rect(self.food.x, self.food.y, BLOCK_SIZE, BLOCK_SIZE))text = font.render("Score: " + str(self.score), True, WHITE)self.display.blit(text, [0, 0])pygame.display.flip()def _move(self, direction):x = self.head.xy = self.head.yif direction == Direction.RIGHT:x += BLOCK_SIZEelif direction == Direction.LEFT:x -= BLOCK_SIZEelif direction == Direction.DOWN:y += BLOCK_SIZEelif direction == Direction.UP:y -= BLOCK_SIZEself.head = Point(x, y)if __name__ == '__main__':game = Snake()# game loopwhile True:game_over, score = game.play_step()if game_over == True:breakprint('Final Score', score)pygame.quit()
```

这是游戏的代码。让我们走一遍

```
class Direction(Enum):RIGHT = 1LEFT = 2UP = 3DOWN = 4
```

我们首先实例化一个方向类来简化用四个基本方向改变蛇的方向的过程。游戏的其余部分用碰撞检测系统就很容易理解了。

```
def _is_collision(self):# hits boundaryif self.head.x > self.w - BLOCK_SIZE or self.head.x < 0 or self.head.y > self.h - BLOCK_SIZE or self.head.y < 0:return True# hits itselfif self.head in self.snake[1:]:return Truereturn False
```

检查蛇是否离开了地图，或者查看蛇的头部是否撞到了自己的头部以外的点。它还实现了一个非常简单的移动系统

```
def _move(self, direction):x = self.head.xy = self.head.yif direction == Direction.RIGHT:x += BLOCK_SIZEelif direction == Direction.LEFT:x -= BLOCK_SIZEelif direction == Direction.DOWN:y += BLOCK_SIZEelif direction == Direction.UP:y -= BLOCK_SIZEself.head = Point(x, y)
```

它只是更新蛇的头部和身体，并根据蛇的当前方向向左、向右、向上或向下移动身体。通过 pygames 内置输入系统实现的简单按键系统有助于改变方向

```
for event in pygame.event.get():if event.type == pygame.QUIT:pygame.quit()quit()if event.type == pygame.KEYDOWN:if event.key == pygame.K_LEFT:if self.direction == Direction.RIGHT:self.direction = Direction.RIGHTelse:self.direction = Direction.LEFTtime.sleep(0.03)elif event.key == pygame.K_RIGHT:if self.direction == Direction.LEFT:self.direction = Direction.LEFTelse:self.direction = Direction.RIGHTtime.sleep(0.03)elif event.key == pygame.K_UP:if self.direction == Direction.DOWN:self.direction = Direction.DOWNelse:self.direction = Direction.UPtime.sleep(0.03)elif event.key == pygame.K_DOWN:if self.direction == Direction.UP:self.direction = Direction.UPelse:self.direction = Direction.DOWNtime.sleep(0.03)
```

我们还添加了一个 caviet，它可以防止我们朝着与 PE 没有实现的方向相反的方向前进。我们还添加了一个 0.03 秒的 time.sleep()来防止快速按键意外杀死播放器，PE 也没有实现这个功能。这就是游戏的基础。现在我们来谈谈环境。

```
import pygameimport randomfrom enum import Enumfrom collections import namedtupleimport numpy as nppygame.init()font = pygame.font.Font('arial.ttf', 25)#font = pygame.font.SysFont('arial', 25)class Direction(Enum):RIGHT = 1LEFT = 2UP = 3DOWN = 4Point = namedtuple('Point', 'x, y')# rgb colorsWHITE = (255, 255, 255)RED = (200,0,0)BLUE1 = (0, 0, 255)BLUE2 = (0, 100, 255)BLACK = (0,0,0)BLOCK_SIZE = 20SPEED = 100class SnakeGameAI:def __init__(self, w=640, h=480):self.w = wself.h = h# init displayself.display = pygame.display.set_mode((self.w, self.h))pygame.display.set_caption('Snake')self.clock = pygame.time.Clock()self.reset()def reset(self):# init game stateself.direction = Direction.RIGHTself.head = Point(self.w/2, self.h/2)self.snake = [self.head,Point(self.head.x-BLOCK_SIZE, self.head.y),Point(self.head.x-(2*BLOCK_SIZE), self.head.y)]self.score = 0self.food = Noneself._place_food()self.frame_iteration = 0def _place_food(self):x = random.randint(0, (self.w-BLOCK_SIZE )//BLOCK_SIZE )*BLOCK_SIZEy = random.randint(0, (self.h-BLOCK_SIZE )//BLOCK_SIZE )*BLOCK_SIZEself.food = Point(x, y)if self.food in self.snake:self._place_food()def play_step(self, action):self.frame_iteration += 1# 1\. collect user inputfor event in pygame.event.get():if event.type == pygame.QUIT:pygame.quit()quit()# 2\. moveself._move(action) # update the headself.snake.insert(0, self.head)# 3\. check if game overreward = 0game_over = Falseif self.is_collision() or self.frame_iteration > 100*len(self.snake):game_over = Truereward = -10return reward, game_over, self.score# 4\. place new food or just moveif self.head == self.food:self.score += 1reward = 10self._place_food()else:self.snake.pop()# 5\. update ui and clockself._update_ui()self.clock.tick(SPEED)# 6\. return game over and scorereturn reward, game_over, self.scoredef is_collision(self, pt=None):if pt is None:pt = self.head# hits boundaryif pt.x > self.w - BLOCK_SIZE or pt.x < 0 or pt.y > self.h - BLOCK_SIZE or pt.y < 0:return True# hits itselfif pt in self.snake[1:]:return Truereturn Falsedef _update_ui(self):self.display.fill(BLACK)for pt in self.snake:pygame.draw.rect(self.display, BLUE1, pygame.Rect(pt.x, pt.y, BLOCK_SIZE, BLOCK_SIZE))pygame.draw.rect(self.display, BLUE2, pygame.Rect(pt.x+4, pt.y+4, 12, 12))pygame.draw.rect(self.display, RED, pygame.Rect(self.food.x, self.food.y, BLOCK_SIZE, BLOCK_SIZE))text = font.render("Score: " + str(self.score), True, WHITE)self.display.blit(text, [0, 0])pygame.display.flip()def _move(self, action):# [straight, right, left]clock_wise = [Direction.RIGHT, Direction.DOWN, Direction.LEFT, Direction.UP]idx = clock_wise.index(self.direction)if np.array_equal(action, [1, 0, 0]):new_dir = clock_wise[idx] # no changeelif np.array_equal(action, [0, 1, 0]):next_idx = (idx + 1) % 4new_dir = clock_wise[next_idx] # right turn r -> d -> l -> uelse: # [0, 0, 1]next_idx = (idx - 1) % 4new_dir = clock_wise[next_idx] # left turn r -> u -> l -> dself.direction = new_dirx = self.head.xy = self.head.yif self.direction == Direction.RIGHT:x += BLOCK_SIZEelif self.direction == Direction.LEFT:x -= BLOCK_SIZEelif self.direction == Direction.DOWN:y += BLOCK_SIZEelif self.direction == Direction.UP:y -= BLOCK_SIZEself.head = Point(x, y)
```

env 与游戏非常相似，但有几个额外的函数使算法能够学习，包括 play_step 函数、reset 函数和 _move 函数。play_step 函数处理在 move 函数之后发生的所有更新，move 函数采用一维数组[1，0，0]、[0，1，0]或[0，0，1]，控制代理是顺时针、逆时针移动还是不执行任何操作。这些操作在 _move 函数中执行。接下来我们来看代理

```
import torchimport randomimport numpy as npfrom collections import dequefrom env import SnakeGameAI, Direction, Pointfrom model import Linear_QNet, QTrainerMAX_MEMORY = 100_000BATCH_SIZE = 1000LR = 0.001class Agent:def __init__(self):self.n_games = 0self.epsilon = 0 # randomnessself.gamma = 0.9 # discount rateself.memory = deque(maxlen=MAX_MEMORY) # popleft()self.model = Linear_QNet(11, 256, 3)#.to(device=torch.device('cuda:0'))self.trainer = QTrainer(self.model, lr=LR, gamma=self.gamma)#.to(device=torch.device('cuda:0'))def get_state(self, game):head = game.snake[0]point_l = Point(head.x - 20, head.y)point_r = Point(head.x + 20, head.y)point_u = Point(head.x, head.y - 20)point_d = Point(head.x, head.y + 20)dir_l = game.direction == Direction.LEFTdir_r = game.direction == Direction.RIGHTdir_u = game.direction == Direction.UPdir_d = game.direction == Direction.DOWNstate = [# Danger straight(dir_r and game.is_collision(point_r)) or(dir_l and game.is_collision(point_l)) or(dir_u and game.is_collision(point_u)) or(dir_d and game.is_collision(point_d)),# Danger right(dir_u and game.is_collision(point_r)) or(dir_d and game.is_collision(point_l)) or(dir_l and game.is_collision(point_u)) or(dir_r and game.is_collision(point_d)),# Danger left(dir_d and game.is_collision(point_r)) or(dir_u and game.is_collision(point_l)) or(dir_r and game.is_collision(point_u)) or(dir_l and game.is_collision(point_d)),# Move directiondir_l,dir_r,dir_u,dir_d,# Food locationgame.food.x < game.head.x, # food leftgame.food.x > game.head.x, # food rightgame.food.y < game.head.y, # food upgame.food.y > game.head.y # food down]return np.array(state, dtype=int)def remember(self, state, action, reward, next_state, done):self.memory.append((state, action, reward, next_state, done)) # popleft if MAX_MEMORY is reacheddef train_long_memory(self):if len(self.memory) > BATCH_SIZE:mini_sample = random.sample(self.memory, BATCH_SIZE) # list of tupleselse:mini_sample = self.memorystates, actions, rewards, next_states, dones = zip(*mini_sample)self.trainer.train_step(states, actions, rewards, next_states, dones)#for state, action, reward, nexrt_state, done in mini_sample:# self.trainer.train_step(state, action, reward, next_state, done)def train_short_memory(self, state, action, reward, next_state, done):self.trainer.train_step(state, action, reward, next_state, done)def get_action(self, state):# random moves: tradeoff exploration / exploitationself.epsilon = 80 - self.n_gamesfinal_move = [0,0,0]if random.randint(0, 200) < self.epsilon:move = random.randint(0, 2)final_move[move] = 1else:state0 = torch.tensor(state, dtype=torch.float)prediction = self.model(state0)move = torch.argmax(prediction).item()final_move[move] = 1return final_movedef train():record = 0agent = Agent()game = SnakeGameAI()while True:# get old statestate_old = agent.get_state(game)# get movefinal_move = agent.get_action(state_old)# perform move and get new statereward, done, score = game.play_step(final_move)state_new = agent.get_state(game)# train short memoryagent.train_short_memory(state_old, final_move, reward, state_new, done)# rememberagent.remember(state_old, final_move, reward, state_new, done)if done:# train long memory, plot resultgame.reset()agent.n_games += 1agent.train_long_memory()if score > record:record = scoreagent.model.save()print('Game', agent.n_games, 'Score', score, 'Record:', record)if __name__ == '__main__':train()
```

关于环境，请参考 PE 的视频，因为他用一种美丽的方式解释说，我不能用文字复制。接下来我们可以继续讨论模型，我发现 PE 的解释中缺少这一部分。这背后的代码是

```
import torchimport torch.nn as nnimport torch.optim as optimimport torch.nn.functional as Fimport osclass Linear_QNet(nn.Module):def __init__(self, input_size, hidden_size, output_size):super().__init__()self.linear1 = nn.Linear(input_size, hidden_size)self.linear2 = nn.Linear(hidden_size, output_size)def forward(self, x):x = F.relu(self.linear1(x))x = self.linear2(x)return xdef save(self, file_name='model.pth'):model_folder_path = './model'if not os.path.exists(model_folder_path):os.makedirs(model_folder_path)file_name = os.path.join(model_folder_path, file_name)torch.save(self.state_dict(), file_name)class QTrainer:def __init__(self, model, lr, gamma):self.lr = lrself.gamma = gammaself.model = modelself.optimizer = optim.Adam(model.parameters(), lr=self.lr)self.criterion = nn.MSELoss()def train_step(self, state, action, reward, next_state, done):state = torch.tensor(state, dtype=torch.float)next_state = torch.tensor(next_state, dtype=torch.float)action = torch.tensor(action, dtype=torch.long)reward = torch.tensor(reward, dtype=torch.float)# (n, x)if len(state.shape) == 1:# (1, x)state = torch.unsqueeze(state, 0)next_state = torch.unsqueeze(next_state, 0)action = torch.unsqueeze(action, 0)reward = torch.unsqueeze(reward, 0)done = (done, )# 1: predicted Q values with current statepred = self.model(state)target = pred.clone()for idx in range(len(done)):Q_new = reward[idx]if not done[idx]:Q_new = reward[idx] + self.gamma * torch.max(self.model(next_state[idx]))target[idx][torch.argmax(action[idx]).item()] = Q_new# 2: Q_new = r + y * max(next_predicted Q value) -> only do this if not done# pred.clone()# preds[argmax(action)] = Q_newself.optimizer.zero_grad()loss = self.criterion(target, pred)loss.backward()self.optimizer.step()
```

该模型是在 pytorch 中实现的一个非常轻量级的线性模型，py torch 是一个非常通用的机器学习库。我选择 pytorch，而不是我通常选择的 tensorflow，因为与 pytorch 模型相比，实现 pytorch 模型很简单。该模型接受 11 个值作为输入，具有 256 个节点作为隐藏层，并输出从 0 到 2 的值作为输出。我们用这几行代码把每个参数转换成张量

```
state = torch.tensor(state, dtype=torch.float)next_state = torch.tensor(next_state, dtype=torch.float)action = torch.tensor(action, dtype=torch.long)reward = torch.tensor(reward, dtype=torch.float)
```

并且这些参数中的每一个然后被取消排队以使它们与神经网络的输入层兼容，并且 done 参数被转换成具有一个元素的元组

```
state = torch.unsqueeze(state, 0)next_state = torch.unsqueeze(next_state, 0)action = torch.unsqueeze(action, 0)reward = torch.unsqueeze(reward, 0)done = (done, )
```

完成所有这些后，我们计算预测结果和模型提供的结果，并使用它们计算损失。

```
Q_new = reward[idx]if not done[idx]:Q_new = reward[idx] + self.gamma * torch.max(self.model(next_state[idx]))target[idx][torch.argmax(action[idx]).item()] = Q_new# 2: Q_new = r + y * max(next_predicted Q value)
```

之后，我们简单地优化模型及其参数

```
self.optimizer.zero_grad()loss = self.criterion(target, pred)loss.backward()self.optimizer.step()
```

这种模型和训练方法产生如下结果:在 30 分钟的训练后完成了[https://youtu.be/kxFVMeoqzkM](https://youtu.be/kxFVMeoqzkM)。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) 

[**成为作家**](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)