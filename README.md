# Group-02
## Coriolis Force simulation

## I. Members
1. 楊添福 106022106
2. 謝明儒 106022215
3. 鄭銘健 106022162
4. 黃景岳 106022225

## II. Goals
To simulate the movement of a throwing ball on a rotating system. (eg. Earth)

## III. Steps
1. Depicting the ball and the rotating planet at initial condition.

2. Calculate the Coriolis force acting on the ball.

3. Present the simulation by a video.

## IV. Assignment
1. 楊添福 -> Formulate the Coriolis force acting on the ball.

2. 黃景岳 -> Find out the solution of the movement equation.

3. 謝明儒 -> Export the animation of the throwing ball.

4. 鄭銘健 -> Debug and check thework of vpython code.

## V. Brief Introduction
Coriolis force is an inertial force acting on objects that are in motion within a frame of reference which rotating with respect to an inertial frame. In a reference frame with clockwise rotation mode, the force acts to the left of the motion of the object; in one with counterclockwise rotation mode, the force acts to the right of the motion of the object.

## VI. Work Log
Week 1:
1. Discuss and organize our final project topic and goal.

Week 2:
1. Install vpython and look out the difference of it.
2. Formulate the motion of the throwing ball.
3. Solve the equation and observe how it moves.

Week 3:
1. Define all the constants we need in this project.
2. Set up input format, containg latitude, velocity, and duration.
3. Generate two windows to show our later detail works.

Week 4:
1. Import the figure of Earth to be as our rotating system.
2. Let the Earth rotates at constant speed and connects to the another window.
3. Set the original frame of observer to be from the outer space.

Week 5:
1. Fire a ball and draw the trajectory 3-D on the main window and the other.
2. Set up a control to save all the datas to check whether it's correct or not.
3. Set a circular plane as the Great circle where the throwing ball only due to force of gravity.

Week 6:
1. Continue our work last week and connects the data simutaneously with another window.
2. Advance our code in order to fire the ball more than one time.
3. Add some extra controls, such as changing fire angle and the frame of observer.

Week 7:
1. Check our progress whether it suits the reality.
2. Beatify our video and refine our vpython code.

Week 8:
1. Double check our code and finish our github and PPT.

## VII. Progress
We first type the ball's information to be latitude: 30 degree, initial velocity: 25000 meter per second, and duration: 3 seconds. After few seconds, we'll get two windows, one with Earth and ball's information at the left side, and the other showing where the ball will land after firing. We click the main window, and see the Earth start to rotate at fixed velocity. We click again and observe that a firing ball due to not only gravity but Coriolis force fly northward initially. Ultimately, it lands at the right side of the firing direction, namely eatward. If we want to fire another ball with dirrerent information, such as direction and angle, we just need to press our keyboard and then click again. The results fit the theory and we successfully simulate the movement of a throwing ball on a rotating system.

## Code
from visual import*
from visual.graph import*
from random import*
import csv, os

#修改項目
#自訂參數 

#A func
g = vector(0.0, -9.8, 0.0)

def count_v(dt, pos):
    if len(pos) == 2:	
        return (pos[1] - pos[0]) / dt

def count_a(dt, pos):
    if len(pos) == 3:
        return (pos[2] + pos[0] - 2 * pos[1]) / dt**2

def get_g(mass, G = g):
    return mass * G

def spring_f(length, k, initlen = 0.0):
    return -norm(length) * k * (mag(length) - mag(initlen))

def update(dt, scene):
    for i in scene.objects:
        if hasattr(i, 'a') and hasattr(i, 'v'):
            i.v += i.a * dt
        if hasattr(i, 'v') and hasattr(i, 'pos'):
            i.pos += i.v *dt
        if hasattr(i, 'v') and hasattr(i, 'S'):
            i.S += mag(i.v *dt)
#B output
def save_csv(filename, data):
    if os.path.exists(".\\" + filename):
        n = 1
        filename = filename.split('.')[0]
        while os.path.exists(".\\" + filename + "(" + str(n) + ").csv"):
            n += 1
        filename = filename + "(" + str(n) + ").csv"

    csvfile = open(filename, "w")
    csvwriter = csv.writer(csvfile, lineterminator = "\n")
    csvwriter.writerows(data)
            
#C Units: km, hr, radian

latitude = float(raw_input("latitude : "))  #in degrees
degree          = 0.26251614                #Earth's rotation speed 角頻率
rotate_ratio10  = 10                        #旋轉速度10
Er              = 6371                      #地球半徑(6371)
fire_angle      = 30                        #發射仰角
fire_dir        = 90                         #北方 (東方為零度)
balls_v         = float(raw_input("initial velocity : "))    #質點速度25000
balls_duration  = float(raw_input("duration : "))     #飛行時間(小時)

#自轉周期
w = vector(0, degree * rotate_ratio10/10, 0)

#E 標示南北半球
if latitude >= 0:
    str_latitude = str(latitude)  + " N"
else:
    str_latitude = str(-latitude) + " S"

#F gn = GM (公里/小時^2)
gn = 9.80665*(Er**2)*(3600**2)*(10**-3)

def gravity(r):
    return -gn/(mag(r)**2) * norm(r)

#G 操作說明
print "\n Units: km, hr\n"
print "Controlings:\n"
print "左右增加旋轉速度      left , right : change rotation speed\n "
print "w : 增加發射仰角     raise throwing angle\n "
print "s : 減少發射仰角     lower throwing angle\n "
print "a : 發射方位減少     throwing direction turns left\n "
print "d : 發射方位增加     throwing direction turns right\n"
print "i : 視角隨地球移動    camera rotates with Earth\n "
print "o : 視角固定         camera sets still\n "
print "b : 視角隨球移動      camera follows flying balls\n "
print "r : 儲存質點數據      save balls data"
print "\nclick to throw the ball\n\n*You can't change rotation speed while any ball exists."

#H 延遲五秒
sleep(5)

#I 畫右上角附圖(圖2)
g_dev = gdisplay(x = 900, y = 0, width = 350, height = 350, title = "Deviation", xtitle = "t", ytitle = "%")
gdots(gdisplay = g_dev, 
#點點圖，設定xy座標，顏色
pos = [(-0.25, 0), (balls_duration, 0), (0, 0.005)], color = color.white, size = 0.01) 


#J 畫右下角附圖(圖3)
g_trail = display(x = 900, y = 350, width = 350, height = 350, center = vector(0, 0, 0), background = color.black, title = "經度緯度關係圖", userspin = False,
                    lights = [distant_light(direction = vector(0, 0, 1)), distant_light(direction = vector(0, 0, -1))])

#K   g_dev
def project(lat, lon, z):
    return vector(Er*lon, Er*lat, z)

curve(display = g_trail, pos = [project(-pi/2, -pi, 0), project(pi/2, -pi, 0), project(pi/2, pi, 0),
                            project(-pi/2, pi, 0), project(-pi/2, -pi, 0)], color = color.gray(0.8), size = 3)

#in range (切割圖3)
for lat in range(-90, 91, 30):
    curve(display = g_trail, pos = [project(radians(lat), -pi, 0), project(radians(lat), pi, 0)], color = color.gray(0.5), size = 3)
    label(display = g_trail, text = str(lat), pos = project(radians(lat), -pi*0.9, 0), box = False, line = False, opacity = 0)
for lon in range(-180, 181, 30):
    curve(display = g_trail, pos = [project(pi/2, radians(lon), 0), project(-pi/2, radians(lon), 0)], color = color.gray(0.5), size = 1.5)

#畫標題"latitude"(vector調位置)
label(display = g_trail, text = "latitude", pos = project(pi/2, -pi, 0) + vector(3500, 3000, 0), box = False, line = False, opacity = 0)                        
[label(display = g_trail, text = str(lat), pos = project(radians(lat), -pi*0.9, 0), box = False, line = False, opacity = 0) for lat in range(-90, 91, 30)]

#NC環畫在圖3 #retain: trai中欲保留的點數
updater = sphere(display = g_trail, radius = 1, color = color.white, make_trail = True, retain = 1050, trail_type = "points")                                   
updater.trail_object.size = 1

#畫主圖(圖1) #V python light 語法 調整照光方向及遠近 #https://www.glowscript.org/docs/VPythonDocs/lights.html

scene = display(width = 900, height = 700, center = vector(0, 0, 0), background = color.black, title = "Throw Ball on Earth",
                lights = [distant_light(direction = vector(0, 1, 0), color = color.gray(0.7)),
                          distant_light(direction = vector(0, -1, 0), color = color.gray(0.7)),
                          distant_light(direction = vector(1, 0, 0), color = color.gray(0.7)),
                          distant_light(direction = vector(-1, 0, 0), color = color.gray(0.7)),
                          distant_light(direction = vector(0, 0, 1), color = color.gray(0.7)),
                          distant_light(direction = vector(0, 0, -1), color = color.gray(0.7))])

#畫底部參考網格
[curve(pos = [vector(4000, -Er*1.2, z*400), vector(-4000, -Er*1.2, z*400)], color = color.gray(0.5)) for z in range(-10, 11)]
[curve(pos = [vector(x*400, -Er*1.2, 4000), vector(x*400, -Er*1.2, -4000)], color = color.gray(0.5)) for x in range(-10, 11)]

#"標題" #Vpython label語法 https://vpython.org/contents/docs/label.html
timer = label(text = "Go Go Go", pos = scene.center, yoffset = scene.height/2-100, height = 50, color = color.red, box = False, line = False, opacity = 10)
rota_demo = str(rotate_ratio10/10.0)

#主圖說明欄 \n表換行
info_demo = label(text = "  Rotation Speed(< >):\n    %sx\n  Latitude:\n    %s\n  Fire Angle(w s):\n    %s  deg\n  Fire Direction(a d):\n    %s  deg\n  Earth Radius:\n    %s  km"
                    % (rota_demo, str_latitude, str(fire_angle), str(fire_dir), str(Er)), pos = scene.center, xoffset = -(scene.width/2-180), height = 16, color = color.white, background = color.black, box = False, line = False, opacity = 0.7)

#畫主圖的地球 #material.earth可以改成 material.BlueMarble會有太空效果
#內建 material https://vpython.org/contents/docs/materials.html

earth = frame(pos = vector(0, 0, 0))
sphere(frame = earth, radius = Er, material = materials.earth, opacity = 0.7)

#pyramid語法 https://vpython.org/contents/docs/pyramid.html

player = pyramid(frame = earth, pos = vector(0, Er * sin(radians(latitude)), Er * cos(radians(latitude))), color = color.green, size = vector(100, 100, 100), material = materials.rough)

#norm 取單位向量 #arrow https://vpython.org/contents/docs/arrow.html

player.axis = norm(player.pos)
fireaxis = arrow(frame = earth, pos = player.pos, length = 300, color = color.green, material = materials.rough)

#rotate(angle(),axis=vec(),origin=vec()) https://www.glowscript.org/docs/VPythonDocs/rotation.html

fireaxis.axis = rotate(rotate(rotate(vector(1, 0, 0), angle = radians(fire_angle), axis =  vector(0, -1, 0)), angle = radians(fire_dir), axis = vector(0, 0, 1)), angle = radians(latitude), axis = vector(-1, 0, 0)) * 300

#畫NC環 cylinder https://www.vpython.org/contents/docs/cylinder.html
track = cylinder(frame = earth, pos = vector(0, 0, 0), radius = 1.5*Er, length = 1, axis = norm(cross(player.axis, fireaxis.axis)), color = color.gray(0.7), opacity = 0.1)
update_frame = frame(frame = earth, axis = track.axis)

#設定相機初始位置 display語法 https://vpython.org/contents/docs/display.html

scene.forward = -earth.frame_to_world(player.pos)
scene.autoscale = False

poss = [player.pos, player.pos]
balln = -1
balls = []
arrows = []
balls_pos = []
trails = []
formula_balls = []
formula_arrows = []
balls_data = [[None], ["t"]]+[[i/200.0] for i in range(501)]

#設定滑鼠操作(左鍵發射)
def mouse_method(evt):
    global balln, balls_data
    if evt.click == "left":
        balln += 1
        graph_color = vector(uniform(0.3, 0.8), uniform(0.0, 0.5), uniform(0.3, 0.8))
        
#外太空視角，球的初始條件
        balls.append(sphere(pos = earth.frame_to_world(player.pos), radius = 40, make_trail = True, color = color.red, material = materials.rough, opacity = 0.5,
                            time = 0.0, num = balln, v = count_v(dt, poss) + balls_v * norm(earth.frame_to_world(fireaxis.axis)), a = vector(0, 0, 0), S = 0.0,
                           graph_trail = sphere(display = g_trail, radius = 300, color = color.white, make_trail = True, trail_type = "points", material = materials.rough),
                            deviation = gcurve(gdisplay = g_dev, color = graph_color, dot = True, size = 5, dot_color = graph_color), dev_count = 0,
                            data = [], dotn = 0))
#取最後一項
        balls[-1].a = gravity(balls[-1].pos)
        balls[-1].graph_trail.trail_object.display = g_trail
        balls[-1].graph_trail.trail_object.color = graph_color
        balls[-1].graph_trail.trail_object.size = 2
        
#紅色箭頭
        arrows.append(arrow(frame = earth, pos = balls[-1].pos, shaftwidth = 20, axis = vector(0, 0, 0), color = color.red, material = materials.rough, opacity = 0.5))
        balls_pos.append([balls[-1].pos*1, balls[-1].pos*1])
        trails.append(curve(frame = earth, pos = [earth.world_to_frame(balls[-1].pos) for i in range(3)], color = graph_color))

#在旋轉坐標裏的球，初始條件
        formula_balls.append(sphere(frame = earth, pos = player.pos, radius = 40, make_trail = False, color = color.blue, material = materials.rough, opacity = 0.5,
                                    v = balls_v * norm(fireaxis.axis), a = vector(0, 0, 0)))
                                    
#解科氏力
        formula_balls[-1].a = gravity(formula_balls[-1].pos) - 2 * cross(w, formula_balls[-1].v) -              
                                          cross(w, cross(w, formula_balls[-1].pos))

        formula_arrows.append(arrow(frame = earth, pos = formula_balls[-1].pos, shaftwidth = 20, axis = vector(0, 0, 0), color = color.blue, material = materials.rough, opacity = 0.5))
        
        balls[-1].data.append([balls[-1].v, balls[-1].a, "NO_DATA", "NO_DATA"])
        balls_data[0] += ["ball "+str(balln+1), "", "", ""]
        balls_data[1] += ["iner_v", "iner_a", "non-iner_v", "non-iner_a"]
        for i in range(501):
            balls_data[i+2] += [None, None, None, None]

#設定鍵盤操作
#切換觀察位置
def key_method(evt):
    global mode, balls_data, degree, rotate_ratio10, w, rota_demo, fire_angle, fire_dir
    k = evt.key
    if k == "i":
        mode = "inside"
        scene.center = timer.pos = info_demo.pos = vector(0, 0, 0)
    elif k == "o":
        mode = "outside"
        scene.center = timer.pos = info_demo.pos = vector(0, 0, 0)
        scene.forward = -earth.frame_to_world(player.pos)
    elif k == "b":
        mode = "ball"
    elif k == "r":
        save_csv("Earth_throwball.csv", balls_data)
	#切換自轉速度
    elif k == "left" and len(balls) == 0:
        rotate_ratio10 -= 1
        w = vector(0, degree * (rotate_ratio10/10.0), 0)
        rota_demo = str(rotate_ratio10/10.0)
    elif k == "right" and len(balls) == 0:
        rotate_ratio10 += 1
        w = vector(0, degree * (rotate_ratio10/10.0), 0)
        rota_demo = str(rotate_ratio10/10.0)
        
#調整下一個飛彈的發射視角
    elif k == "w" or k == "s" or k == "a" or k == "d":
        if k == "w" and fire_angle < 90:
            fire_angle += 1
        elif k == "s" and fire_angle > 0:
            fire_angle -= 1
        elif k == "a":
            fire_dir += 1
            fire_dir %= 360
        elif k == "d":
            fire_dir -=1
            fire_dir %= 360
        fireaxis.axis = rotate(rotate(rotate(vector(1, 0, 0), angle = radians(fire_angle), axis = vector(0, -1, 0)), angle = radians(fire_dir), axis = vector(0, 0, 1)), angle = radians(latitude), axis = vector(-1, 0, 0)) * 300
        if not fire_angle == 90:
            update_frame.axis = track.axis = norm(cross(player.axis, fireaxis.axis))
            
#主圖說明欄調整
    info_demo.text = ("  Rotation Speed(< >):\n    %sx\n  Latitude:\n    %s\n  Fire Angle(w s):\n    %s  deg\n  Fire Direction(A d):\n    %s  deg\n  Earth Radius:\n    %s  km"
                        % (rota_demo , str_latitude, str(fire_angle), str(fire_dir), Er))
#o模式 視角由發射點開始
mode = "outside"
t = 0
dt = 0.0001

#畫時間(點擊後開始)
scene.waitfor("click")
scene.bind("click", mouse_method)
scene.bind("keydown", key_method)
timer.color = color.yellow

while True:
    rate(0.1/dt)
    
    t += dt
    timer.text = str(int(t*10)/10.0)+" hr"

#設定時間跟說明欄的位置
    timer.yoffset = scene.height/2-100
    info_demo.xoffset = -(scene.width/2-180)

    tmp_pos = update_frame.frame_to_world(vector(0, sin(-t*60)*Er, cos(-t*60)*Er))
    updater_lat = asin(tmp_pos.y/Er)
    if tmp_pos.z > 0:
        updater_lon = atan(tmp_pos.x/tmp_pos.z)
    else:
        if tmp_pos.x > 0:
            updater_lon = pi + atan(tmp_pos.x/tmp_pos.z)
        else:
            updater_lon = -pi + atan(tmp_pos.x/tmp_pos.z)
    updater.pos = project(updater_lat, updater_lon, 1)

    poss[0] = poss[1]*1
    poss[1] = earth.frame_to_world(player.pos)*1
    
    for b in balls:
#ball若飛行時間大於duration或位置小於地半徑則停止紀錄
        if b.time >= balls_duration or mag(b.pos) - Er <= -0.01:
            for i in range(len(b.data)):
                for j in range(4):
                    balls_data[i+2][j+b.num*4+1] = b.data[i][j]
            b.trail_object.visible = False
            b.visible = False
            formula_balls[balls.index(b)].visible = False
            arrows[balls.index(b)].visible = False
            formula_arrows[balls.index(b)].visible = False
            del b.deviation, b.graph_trail, balls_pos[balls.index(b)], trails[balls.index(b)], formula_arrows[balls.index(b)], formula_balls[balls.index(b)], arrows[balls.index(b)], balls[balls.index(b)]
#否則繼續記錄數據        
        else:
            b.time += dt
            b.dotn += 1
            ballpos = earth.world_to_frame(b.pos)
            b.a = gravity(b.pos)
            balls_pos[balls.index(b)].append(b.pos*1)
            trails[balls.index(b)].append(pos = ballpos)

            #deviation(圖2)公式
            if b.S:
                b.deviation.plot(pos = (b.time, mag(ballpos - formula_balls[balls.index(b)].pos)*100 / b.S))
            
            updater_lat = asin(ballpos.y/mag(ballpos))
            if ballpos.z > 0:
                updater_lon = atan(ballpos.x/ballpos.z)
            else:
                if ballpos.x > 0:
                    updater_lon = pi + atan(ballpos.x/ballpos.z)
                else:
                    updater_lon = -pi + atan(ballpos.x/ballpos.z)
            b.graph_trail.pos = project(updater_lat, updater_lon, 1)

            if not(b.dotn % 50):
                b.data.append([count_v(dt, balls_pos[balls.index(b)][-2:]),
                               count_a(dt, balls_pos[balls.index(b)][-3:]) - gravity(balls_pos[balls.index(b)][-2]),
                               vector(count_v(dt, trails[balls.index(b)].pos[-2:])),
                               vector(count_a(dt, trails[balls.index(b)].pos[-3:])) - gravity(vector(trails[balls.index(b)].pos[-2])) ])
            arrows[balls.index(b)].pos = ballpos
            arrows[balls.index(b)].axis = (vector(count_a(dt, trails[balls.index(b)].pos[-3:]))
                                            - gravity(vector(trails[balls.index(b)].pos[-2]))) * 0.03
#科氏力
    for fb in formula_balls:
        fb.a = gravity(fb.pos) - 2 * cross(w, fb.v) - cross(w, cross(w, fb.pos))
        formula_arrows[formula_balls.index(fb)].pos = fb.pos
        formula_arrows[formula_balls.index(fb)].axis = (fb.a - gravity(fb.pos)) * 0.03
    
    earth.rotate(angle = mag(w) * dt, axis = norm(w))
    update(dt, scene)
    
#視角隨地球移動
    if mode == "inside":
        scene.forward = -earth.frame_to_world(player.pos)
    elif mode == "ball" and len(balls):
        scene.center = timer.pos = info_demo.pos = balls[-1].pos
        scene.forward = -balls[-1].pos
