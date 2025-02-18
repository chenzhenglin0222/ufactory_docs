# WebsocketAPI

Set motion(ws://{ip}:{port}/ws)\\

## 1.xarm\_move\_step

Description:\
Long press: keep moving in a certain direction until the button is released.\
Short press: move a step in a certain direction.\
Button: \[Live Control] -- X+ / X- / Y+ / Y- / Z+ / Z-\\

```
URL:  /ws 
Request Type： Application/json 
Return Type： */*  
```

<table><thead><tr><th width="196">Request Parameter</th><th width="86">Type</th><th width="138">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>isloop</td><td>bool</td><td>Yes</td><td>whether a long press or not</td></tr><tr><td>direction</td><td>String</td><td>Yes</td><td>Direction of movement：<br>'position-x-increase’;'position-x-decrease’;<br>’position-y-increase’;’position-y-decrease’;<br>’position-z-increase’;’position-z-decrease’;<br>’attitude-roll-increase’;'attitude-roll-decrease';<br>’attitude-pitch-increase’;'attitude-pitch-decrease';<br>'attitude-yaw-increase';'attitude-yaw-decrease';<br>Change of Joint angle：<br>'joint-angle-increase';'joint-angle-decrease';<br>Aligning the Hand(for xArm5 only)：<br>'set-end-level' ;</td></tr><tr><td>isMoveTool</td><td>bool</td><td>Yes</td><td>whether use Tool Coordinate or not(Base Coordinate by default)</td></tr><tr><td>isSetInitialPoint</td><td>bool</td><td>No</td><td>(Not Used)</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
if direction == 'position-x-increase' or direction == 'position-x-decrease':
    self._xarm_sync_tcp(0)
    x = GLOBAL.XArm.xarm_position_step if direction == 'position-x-increase' else -GLOBAL.XArm.xarm_position_step
    if isMoveTool:
        code = GLOBAL.XArm.xarm.set_tool_position(x=x, speed=tcp_speed)
    else:
        code = GLOBAL.XArm.xarm.set_position(x=x, relative=True, speed=tcp_speed)
```

### Front end

```javascript
moveStep(direction, isLoop) {
      window.CommandsRobotSocket.moveStep({ 'direction': direction, 'isLoop': isLoop, 'isMoveTool': this.isToolCoord });
},

moveStep = (data, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, data);
  Object.assign(params.data, {
    mode: self.model.robot.state.info.xarm_mode,
  });
  self.sendCmd(window.GlobalConstant.MOVE_STEP_START, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
}
```

## 2.xarm\_move\_step\_over

Description:\
The xArm will be stopped immediately once the button is released.

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th>Request Parameter</th><th width="122">Type</th><th>Must be filled</th><th>Description</th></tr></thead><tbody><tr><td><em>/</em></td><td></td><td></td><td></td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0 by default</td></tr></tbody></table>

### Background

```python
if direction == 'position-x-increase' or direction == 'position-x-decrease':
    self._xarm_sync_tcp(0)
    x = GLOBAL.XArm.xarm_position_step if direction == 'position-x-increase' else -GLOBAL.XArm.xarm_position_step
    if isMoveTool:
        code = GLOBAL.XArm.xarm.set_tool_position(x=x, speed=tcp_speed)
    else:
        code = GLOBAL.XArm.xarm.set_position(x=x, relative=True, speed=tcp_speed)
```

### Front end

```javascript
moveStep(direction, isLoop) {
      window.CommandsRobotSocket.moveStep({ 'direction': direction, 'isLoop': isLoop, 'isMoveTool': this.isToolCoord });
},

moveStep = (data, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, data);
  Object.assign(params.data, {
    mode: self.model.robot.state.info.xarm_mode,
  });
  self.sendCmd(window.GlobalConstant.MOVE_STEP_START, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
}
```

## 3.xarm\_move\_joint

Description:\
Move with Joint Motion\
1）Back to initial position 2）Joint motion in 'Live Control' interface

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="200">Request Parameter</th><th width="87">Type</th><th width="135">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>I</td><td>String</td><td>Yes</td><td>Joint angle of Joint1</td></tr><tr><td>J</td><td>String</td><td>Yes</td><td>Joint angle of Joint2</td></tr><tr><td>K</td><td>String</td><td>Yes</td><td>Joint angle of Joint3</td></tr><tr><td>M</td><td>String</td><td>Yes</td><td>Joint angle of Joint4</td></tr><tr><td>N</td><td>String</td><td>Yes</td><td>Joint angle of Joint5</td></tr><tr><td>O</td><td>String</td><td>Yes</td><td>Joint angle of Joint6</td></tr><tr><td>R</td><td>String</td><td>Yes</td><td>Joint angle of Joint7</td></tr><tr><td>wait</td><td>String</td><td>No</td><td>wait or not</td></tr><tr><td>isControl</td><td>bool</td><td>Yes</td><td>whether start the program by Blockly or not</td></tr><tr><td>isClickMove</td><td>bool</td><td>Yes</td><td>whether click 'Move' in the Blockly or not</td></tr><tr><td>Module</td><td>String</td><td>No</td><td>whether the wait=True in the Blockly or not</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td><em>/</em></td><td></td><td></td><td></td></tr></tbody></table>

### Background

```python
GLOBAL.XArm.xarm_printed = True
ret = yield self._wait_until_cmdnum_lt_max()
if ret is not None:
    return response(client, id, ret)
self.last_move_client = client
code = GLOBAL.XArm.xarm.set_servo_angle(angle=angles, speed=mvvelo, mvacc=mvacc, mvtime=mvtime,is_radian=False, wait=False, radius=radius)
```

### Front end

```javascript
self.moveJoint = (positions, index, isWait, callback, isControl, modName, isClickMove) => {
  if (modName === 'blockly' && isControl === false && !window.Blockly.Running) {
    return;
  }
  const JOINT_LIST = ['I', 'J', 'K', 'L', 'M', 'N', 'O', 'R'];
  const sendData = isWait === true ? { wait: true } : {};
  if (index >= 0) {
    sendData[JOINT_LIST[index]] = Number(positions[0]);
  }
  else {
    JOINT_LIST.forEach((value, index) => {
      sendData[value] = Number(positions[index]);
    });
  }
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    F: Number(self.model.robot.state.local.angle_speed),
    Q: Number(self.model.robot.state.local.angle_acceleration),
    isControl: isControl === true ? true : false,
    module: modName,
    isClickMove: isClickMove,
    mode: self.model.robot.state.info.xarm_mode,
    wait: isWait,
  });
  Object.assign(params.data, sendData);

  self.sendCmd(window.GlobalConstant.MOVE_JOINT, params, (dict) => {
    self.codeHandle(dict, 'move joint', isControl !== true);
    if (callback) {
      callback(dict);
    }
  });
};
```

## 4.switch\_mode\_Lite6

Description:\
Change the mode of Lite6. Button: \[Live Control] - Manual mode

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="211">Request Parameter</th><th width="81">Type</th><th width="143">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>mode</td><td>int</td><td>Yes</td><td>0:position mode;<br>2:joint teaching mode</td></tr><tr><td>version</td><td>String</td><td>Yes</td><td>Joint angle of Joint2</td></tr><tr><td>status</td><td>int</td><td>Yes</td><td>0:close joint teaching mode;<br>1:open joint teaching mode;</td></tr><tr><td>Reported</td><td></td><td></td><td></td></tr><tr><td>Lite6_record_mode</td><td>int</td><td>Yes</td><td>return 1 if the teaching mode is enabled;<br>return 0 if the teaching mode is closed.</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td><em>/</em></td><td></td><td></td><td></td></tr></tbody></table>

### Background

```python
if current_tgpio == '' and value == 1:
    xarm.set_mode(mode, 1)
    if xarm.error_code == 0:
        xarm.set_state(0)
    self._xarm_sync()
elif current_tgpio != value:
    GLOBAL.XArm.lite6_ti2_status = value
    mode_param = (2, 1) if value == 1 else (0,)
    xarm.set_mode(*mode_param)
    if xarm.error_code == 0:
        xarm.set_state(0)
```

### Front end

```javascript
self.switch_mode_lite6 = (mode, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    mode: mode,
    status: mode === 2 ? 1 : 0
  });
  self.sendCmd(window.GlobalConstant.SWITCH_MODE_LITE6, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
};
```

## 5.xarm\_set\_blockly\_init

Description:\
Initialize the parameter of Lite6 before runing Blockly project.

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="197">Request Parameter</th><th width="107">Type</th><th width="159">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>version</td><td>String</td><td>Yes</td><td>Lite by default</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0 by default</td></tr></tbody></table>

### Background

```python
mode = GLOBAL.XArm.xarm.mode
if mode == 2:
    return response(client, cmd_id, 105)
yield self.xarm_check_tcp(None, cmd_id, data)
yield self.xarm_set_ready(None, cmd_id, data)
self._xarm_set_params(**{
    'Q': GLOBAL.XArm.xarm_tcp_acc,
    'Q2': GLOBAL.XArm.xarm_joint_acc,
})
```

### Front end

```javascript
self.setBlocklyInit = (callback) => {
  window.GlobalUtil.model.robot.event.GPIOEvent.reset(true);
  window.GlobalUtil.model.robot.state.local.acceleration = window.GlobalUtil.model.robot.state.remote.defaultTcpAcc;
  window.GlobalUtil.model.robot.state.local.angle_acceleration = window.GlobalUtil.model.robot.state.remote.defaultJointAcc;
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    mode: self.model.robot.state.info.xarm_mode,
  });
  self.sendCmd(window.GlobalConstant.SET_BLOCKLY_INIT, params, (dict) => {
    self.codeHandle(dict, 'set blockly init', true);
    if (callback) {
      callback(dict);
    }
  });
}
```

## 6.run\_blockly

Description:\
Run the Blockly project.

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="221">Request Parameter</th><th width="104">Type</th><th width="164">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>userId</td><td>String</td><td>No</td><td>test by default</td></tr><tr><td>version</td><td>String</td><td>No</td><td>Lite6 by default</td></tr><tr><td>category</td><td>String</td><td>No</td><td>myapp by default</td></tr><tr><td>appName</td><td>String</td><td>Yes</td><td>The name of the Blockly project</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td></td><td>0:success;<br>1:Initialization exception;<br>-12: failed;<br>-11: covert failed;<br>-3: parameter error;</td></tr></tbody></table>

### Background

```python
user_id = data.get('userId', 'test')
xarm_version = data.get('version', GLOBAL.XArm.xarm_type)
category = data.get('category', 'myapp')
app_name = data.get('appName', '')
app_name = convert_path(app_name)
path = os.path.join(projects_path, user_id, xarm_version, 'app', category, app_name)
check_is_pause = GLOBAL.XArm.xarm._arm._check_is_pause
check_cmdnum_limit = GLOBAL.XArm.xarm._arm._check_cmdnum_limit
try:
    GLOBAL.XArm.xarm._arm._check_is_pause = True
    GLOBAL.XArm.xarm._arm._check_cmdnum_limit = True
    logger.info('start run blockly')
    GLOBAL.Connect.blockly_task_client = client
    code = yield self._run_blockly(path, client, cmd_id)
    logger.info('run blockly finish, code={}'.format(code))
except:
    code = 1
finally:
    GLOBAL.XArm.xarm._arm._check_is_pause = check_is_pause
    GLOBAL.XArm.xarm._arm._check_cmdnum_limit = check_cmdnum_limit
    GLOBAL.Connect.blockly_task_client = None
if client:
    response(client, cmd_id, code)
else:
    return code
```

### Front end

```javascript
self.runBlockly = (name, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    category: 'myapp', // 'myapp',
    appName: name,
  });
  window.GlobalUtil.model.localAppsMgr.taskId = self.sendCmd(window.GlobalConstant.RUN_BLOCKLY, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  })
};
```

## 7.xarm\_urgent\_stop

Description:\
Emergency Stop

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="198">Request Parameter</th><th width="126">Type</th><th width="169">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>/</td><td></td><td></td><td></td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0 by default</td></tr></tbody></table>

### Background

```python
GLOBAL.XArm.xarm_paused = False
GLOBAL.XArm.xarm_printed = False
GLOBAL.XArm.xarm_print_client = None
self.move_enable = False
GLOBAL.XArm.xarm_print_send_count = 0
GLOBAL.XArm.xarm_print_total_count = 0
GLOBAL.XArm.xarm.emergency_stop()

if GLOBAL.Core.is_xarm and os.path.exists('/home/uf/.UFACTORY/projects/test/task.pid'):
    try:
        with open('/home/uf/.UFACTORY/projects/test/task.pid', 'r') as f:
            pid = f.read()
            os.system('echo "{}" | sudo -S kill -9 {}'.format(GLOBAL.XArm.get_ssh_config()['password'], pid))
    except Exception as e:
        logger.error('read pid error, {}'.format(e))
    finally:
        try:
            os.remove('/home/uf/.UFACTORY/projects/test/task.pid')
        except Exception as e:
            logger.error('remove pid error, {}'.format(e))

if GLOBAL.Core.jedi_ws and GLOBAL.Core.jedi_ws.connected:
    GLOBAL.Core.jedi_ws.send({
        'id': -1,
        'client_id': client._id(),
        'cmd': 'stop_python_script',
        'data': {}
    })
```

### Front end

```javascript
self.urgentStop = (data, callback) => {
  self.model.robot.state.status.error = data;
  self.model.commonStatusMgr.blocklyCanUpdate = false;
  self.model.commonStatusMgr.UrgentStopRecord = true;
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  self.sendCmd(window.GlobalConstant.URGENT_STOP, params, (response) => {
    if (data === false) {
      window.Blockly.Running = false;
      window.Blockly.AppRunning = false;
      window.Blockly.DisabledToRun = false;

      let len = 0;
      if (BlocklyCom.BlockWorkspace) {
        len = BlocklyCom.BlockWorkspace.getAllBlocks().length;
      }
      window.GlobalUtil.model.localAppsMgr.enableRun = len > 0 ? true : false;

      setTimeout(() => {
        self.model.commonStatusMgr.blocklyCanUpdate = true;
      }, 500);
    }

    const stop = response.data;
    if (stop && (stop.length > 0)) {
      console.log('stop', stop);
    }
    else {
      console.log('urgent stop fail', response);
    }
    if (callback) {
      callback(response);
    }
  });
};
```

## 8.xarm\_set\_simulation\_robot

Description:\
Enable/Disable simulated robot

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="199">Request Parameter</th><th width="77">Type</th><th width="137">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>on_off</td><td>bool</td><td>yes</td><td>True: enable;<br>False: disable</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
on_off = data.get('on_off')
code = GLOBAL.XArm.xarm.set_simulation_robot(on_off)
GLOBAL.XArm.xarm.get_position()
GLOBAL.XArm.xarm.get_servo_angle()
yield self._xarm_sync()
response(client, cmd_id, code)
```

### Front end

```javascript
self.set_simulation_robot = (on_off, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    on_off: on_off,
  });
  self.sendCmd(window.GlobalConstant.SET_SIMULATION_ROBOT, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
}
```

## 9.xarm\_set\_speed

Description:\
Set the speed percent of Arm\
Button:\[Live Control]- Speed

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="200">Request Parameter</th><th width="91">Type</th><th width="144">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>percent</td><td>int</td><td>yes</td><td>set the speed percent of the Arm 0.01-1(1-100%)</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0 by default</td></tr></tbody></table>

### Background

```python
percent = float(data.get('percent'))  # percent
if percent > 1:
    percent = 1
max_tcp_speed = GLOBAL.XArm.xarm_params.get('LIMIT_VELO', GLOBAL.Config.LIMIT_VELO)[1]
# max_joint_speed = GLOBAL.XArm.xarm_params.get('LIMIT_ANGLE_VELO', GLOBAL.Config.LIMIT_ANGLE_VELO)[1]
max_joint_speed = GLOBAL.XArm.xarm_max_joint_speed
tcp_speed = max_tcp_speed * percent * GLOBAL.XArm.xarm_speed_factor
joint_speed = max_joint_speed * percent * GLOBAL.XArm.xarm_speed_factor
GLOBAL.XArm.xarm_speed_percent = percent
GLOBAL.XArm.save()
self._xarm_set_params(**{
    'F': tcp_speed,
    'F2': joint_speed
}, is_radian=False)
GLOBAL.XArm.xarm_params = self._xarm_get_params(is_radian=False)
return response(client, cmd_id, 0, 'ok')
```

### Front end

```javascript
self.setSpeedPercent = (percent, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    percent: percent
  });
  self.sendCmd(window.GlobalConstant.SET_SPEED_PERCENT, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
}
```

## 10.xarm\_set\_lite6\_gripper

Description:\
Open/close/stop the lite6 gripper\
Button:\[Live Control] - \[open/close/stop]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="194">Request Parameter</th><th width="83">Type</th><th width="137">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>op</td><td>String</td><td>yes</td><td>open: Open the gripper Lite;<br>close: Close the gripper Lite;<br>stop: Stop the gripper Lite</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
op = data.get('op')
if op == 'open':
    code = GLOBAL.XArm.xarm.open_lite6_gripper()
    response(client, cmd_id, code)
elif op == 'close':
    code = GLOBAL.XArm.xarm.close_lite6_gripper()
    response(client, cmd_id, code)
elif op == 'stop':
    code = GLOBAL.XArm.xarm.stop_lite6_gripper()
    response(client, cmd_id, code)
```

### Front end

```javascript
type ofself.setLite6Gripper = (op, callback) => {
  // control lite6 gripper
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    op: typeof op === 'function' ? op.name : op,
  });
  self.sendCmd(window.GlobalConstant.XARM_SET_LITE6_GRIPPER, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
}
```

## 11.xarm\_move\_arc\_line

Description:\
Linear Motion\
Button:\[Blockly]- \[linear motion]- \[move]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="197">Request Parameter</th><th width="88">Type</th><th width="137">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>X</td><td>String</td><td>Yes</td><td>X</td></tr><tr><td>Y</td><td>String</td><td>Yes</td><td>Y</td></tr><tr><td>Z</td><td>String</td><td>Yes</td><td>Z</td></tr><tr><td>A</td><td>String</td><td>Yes</td><td>Roll</td></tr><tr><td>M</td><td>String</td><td>Yes</td><td>Pitch</td></tr><tr><td>B</td><td>String</td><td>Yes</td><td>Yaw</td></tr><tr><td>R</td><td>String</td><td>No</td><td>Radius</td></tr><tr><td>relative</td><td>String</td><td>Yes</td><td>whether relative motion or not.<br>True: Yes<br>Flase: No</td></tr><tr><td>wait</td><td>String</td><td>No</td><td>wait or not</td></tr><tr><td>isControl</td><td>bool</td><td>Yes</td><td>whether start the program by Blockly or not</td></tr><tr><td>isClickMove</td><td>bool</td><td>Yes</td><td>whether click 'Move' in the Blockly or not</td></tr><tr><td>module</td><td>String</td><td>No</td><td>whether the blockly project is running or not.</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
code, limit = 0, False
if code == 0 and limit is False:
    if is_control:
        if is_click_move:
            if GLOBAL.XArm.xarm.warn_code != 0:
                return response(client, id, GLOBAL.XArm.xarm.error_code)
            elif GLOBAL.XArm.xarm.error_code != 0:
                # GLOBAL.XArm.xarm.set_mode(4)
                return response(client, id, GLOBAL.XArm.xarm.error_code)
        yield self.xarm_set_ready(None, id, data)
    if GLOBAL.XArm.xarm.error_code != 0:
        code = -1
    else:
        GLOBAL.XArm.xarm_printed = True
        ret = yield self._wait_until_cmdnum_lt_max()
        if ret is not None:
            return response(client, id, ret)
self.last_move_client = client
code = GLOBAL.XArm.xarm.set_position(*pose, radius=radius, speed=mvvelo, mvacc=mvacc, mvtime=mvtime,is_radian=False, relative=relative)
```

### Front end

```javascript
self.moveArcLine = (data, isWait, callback, isControl, modName, isClickMove) => {
  if (modName === 'blockly' && isControl === false && !window.Blockly.Running) {
    return;
  }
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    X: Number(data.position.x || 0),
    Y: Number(data.position.y || 0),
    Z: Number(data.position.z || 0),
    // A: self.model.robot.state.info.xarm_axis === 5 ? 180 : Number(data.orientation.roll || 0),
    // B: self.model.robot.state.info.xarm_axis === 5 ? 0 : Number(data.orientation.pitch || 0),
    A: Number(data.orientation.roll || 0),
    B: Number(data.orientation.pitch || 0),
    C: Number(data.orientation.yaw || 0),
    // R: Number(data.radius || 0),
    R: Number(data.orientation.r || 0),
    F: Number(self.model.robot.state.local.speed || 0),
    Q: Number(self.model.robot.state.local.acceleration || 0),
    wait: isWait,
    isControl: isControl === true ? true : false,
    module: modName,
    isClickMove: isClickMove,
    mode: self.model.robot.state.info.xarm_mode,
  });
  self.sendCmd(window.GlobalConstant.MOVE_ARC_LINE, params, (dict) => {
    self.codeHandle(dict, 'move (arc) line', isControl !== true);
    if (callback) {
      callback(dict);
    }
  });
};
```

## 12.xarm\_move\_circle

Description:\
Move circle\
Button:\[Blockly] - \[move circle] - \[move]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="194">Request Parameter</th><th width="79">Type</th><th width="136">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>module</td><td>String</td><td>No</td><td>whether the blockly project is running or not.</td></tr><tr><td>pose1</td><td>String</td><td>Yes</td><td>The first position: [x(mm), y(mm), z(mm), roll(rad or °), pitch(rad or °), yaw(rad or °)]</td></tr><tr><td>pose2</td><td>String</td><td>Yes</td><td>The second position: [x(mm), y(mm), z(mm), roll(rad or °), pitch(rad or °), yaw(rad or °)]</td></tr><tr><td>percent</td><td>Float</td><td>Yes</td><td>1-100%;<br>percent = center angle/360</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
module = data.get('module', '')
mode = GLOBAL.XArm.xarm.mode
if mode == 2 and module == 'blockly':
    return response(client, cmd_id, 105)
yield self._wait_until_not_pause()
pose1 = data.get('pose1')
pose2 = data.get('pose2')
percent = data.get('percent')
wait = data.get('wait', False)
mvvelo = data.get('F', data.get('f', None))
mvacc = data.get('Q', data.get('q', None))
mvtime = data.get('T', data.get('t', None))
if isinstance(mvvelo, str):
    mvvelo = float(mvvelo)
if isinstance(mvacc, str):
    mvacc = float(mvacc)
if isinstance(mvtime, str):
    mvtime = float(mvtime)
mvvelo = None
mvacc = None
mvtime = 0

if GLOBAL.XArm.xarm.error_code != 0:
    code = -1
else:
    ret = yield self._wait_until_cmdnum_lt_max()
    if ret is not None:
        return response(client, cmd_id, ret)
    GLOBAL.XArm.xarm_printed = True
    self.last_move_client = client
    code = GLOBAL.XArm.xarm.move_circle(pose1=pose1, pose2=pose2, percent=percent,
                                        speed=mvvelo, mvacc=mvacc, mvtime=mvtime, wait=False)
    if code in [0, XCONF.UxbusState.ERR_CODE, XCONF.UxbusState.WAR_CODE] and wait:
        before_wait = time.monotonic()
        code = yield self._wait_move()
        logger.info('WaitMove -> code={} -> waiting_time={} -> state={}'.format(
            code, time.monotonic() - before_wait, GLOBAL.XArm.xarm.state
        ))
        self._xarm_sync()
    GLOBAL.XArm.xarm_printed = False
response(client, cmd_id, code)
```

### Front end

```javascript
self.moveCircle = (pose1, pose2, percent, wait, callback, isControl, module) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    pose1: pose1,
    pose2: pose2,
    percent: percent / 360 * 100,
    wait: wait === true ? true : false,
    module: module,
  });
  self.sendCmd(window.GlobalConstant.MOVE_CIRCLE, params, (dict) => {
    self.codeHandle(dict, 'move circle', isControl !== true);
    if (callback) {
      callback(dict);
    }
  });
};
```

## 13.xarm\_set\_cgpio\_digital

Description:\
Set Controller Digital Output to low/high\
Button: \[Blockly]- \[Controller IO]-\[Set CO]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="191">Request Parameter</th><th width="80">Type</th><th width="140">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>ionum</td><td>int</td><td>Yes</td><td>CO0-CO7</td></tr><tr><td>value</td><td>int</td><td>Yes</td><td>0: low level;<br>1: high level</td></tr><tr><td>delay</td><td>int</td><td>No</td><td>delay time;<br>0: set it at once</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
GLOBAL.XArm.xarm_printed = True
ret = yield self._wait_until_cmdnum_lt_max()
if ret is not None:
    return response(client, cmd_id, ret)
ionum = data.get('ionum')
value = int(data.get('value'))
xyz = data.get('xyz')
tol_r = data.get('tol_r')
code = GLOBAL.XArm.xarm.set_cgpio_digital_with_xyz(ionum, value, xyz, tol_r)
response(client, cmd_id, code)
```

### Front end

```javascript
self.set_cgpio_digital = async (ionum, value, callback, delay) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    ionum: ionum,
    value: value,
    delay: (delay !== undefined && delay > 0) ? delay : 0
  });
  console.log('set_cgpio_digital params', params);
  const cmd_id = self.sendCmd(window.GlobalConstant.SET_CGPIO_DIGITAL, params, (dict) => {
    self.codeHandle(dict, 'set cgpio digital', true);
    if (callback) {
      callback(dict);
    }
  });
  await self.socketCom.wait(cmd_id);
};
```

## 14.xarm\_set\_cgpio\_analog

Description:\
Set Controller Analog Value\
Button: \[Blockly] - \[Controller IO] - \[AO set]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="204">Request Parameter</th><th width="81">Type</th><th width="143">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>ionum</td><td>int</td><td>Yes</td><td>AO0-AO1</td></tr><tr><td>value</td><td>Float</td><td>Yes</td><td>value</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
GLOBAL.XArm.xarm_printed = True
ret = yield self._wait_until_cmdnum_lt_max()
if ret is not None:
    return response(client, cmd_id, ret)
ionum = data.get('ionum')
value = data.get('value')
xyz = data.get('xyz')
tol_r = data.get('tol_r')
code = GLOBAL.XArm.xarm.set_cgpio_analog_with_xyz(ionum, value, xyz, tol_r)
response(client, cmd_id, code)
```

### Front end

```javascript
self.set_cgpio_analog = async (ionum, value, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    ionum: ionum,
    value: value,
  });
  console.log('set_cgpio_analog params', params);
  const cmd_id = self.sendCmd(window.GlobalConstant.SET_CGPIO_ANALOG, params, (dict) => {
    self.codeHandle(dict, 'set cgpio analog', true);
    if (callback) {
      callback(dict);
    }
  });
  await self.socketCom.wait(cmd_id);
};
```

## 15.xarm\_set\_gpio\_digital

Description:\
Set tool digital output.\
Button: \[Blockly] - \[Tool IO] - \[set]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="199">Request Parameter</th><th width="76">Type</th><th width="140">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>ionum</td><td>int</td><td>Yes</td><td>TO0-TO1</td></tr><tr><td>value</td><td>int</td><td>Yes</td><td>0: low level;<br>1: high level</td></tr><tr><td>delay</td><td>int</td><td>No</td><td>delay time;<br>0: set it at once</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
yield self._wait_until_not_pause()
GLOBAL.XArm.xarm_printed = True
ret = yield self._wait_until_cmdnum_lt_max()
if ret is not None:
    return response(client, cmd_id, ret)
ionum = data.get('ionum')
value = data.get('value')
delay = data.get('delay', 0)
code = GLOBAL.XArm.xarm.set_tgpio_digital(ionum, value, delay_sec=delay)
response(client, cmd_id, code)
```

### Front end

```javascript
self.set_gpio_digital = async (ionum, value, callback, delay) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    ionum: ionum,
    value: value,
    delay: (delay !== undefined && delay > 0) ? delay : 0
  });
  const cmd_id = self.sendCmd(window.GlobalConstant.SET_GPIO_DIGITAL, params, (dict) => {
    self.codeHandle(dict, 'set tgpio digital', true);
    if (callback) {
      callback(dict);
    }
  });
  await self.socketCom.wait(cmd_id);
};
```

## 16.xarm\_set\_suction\_cup

Description:\
Open/close the vacuum gripper\
Button: \[Blockly] - \[End effector] - \[set]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="194">Request Parameter</th><th width="76">Type</th><th width="144">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>on</td><td>bool</td><td>Yes</td><td>Open: True;<br>Close: Flase</td></tr><tr><td>wait</td><td>bool</td><td>Yes</td><td>wait or not</td></tr><tr><td>delay</td><td>int</td><td>No</td><td>delay time;<br>0: set it at once</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>Other: Failed. Refer to <a href="https://github.com/xArm-Developer/xArm-Python-SDK/blob/master/doc/api/xarm_api_code.md#api-code">xArm_Python_SDK/xarm_API_Code.md</a></td></tr></tbody></table>

### Background

```python
yield self._wait_until_not_pause()
if self.is_simulation_robot:
    return response(client, cmd_id, 0)
ret = yield self._wait_until_cmdnum_lt_max()
if ret is not None:
    return response(client, cmd_id, ret)
on = data.get('on')
wait = data.get('wait', True)
delay = data.get('delay', 0)
timeout = 3
code = GLOBAL.XArm.xarm.set_suction_cup(on, wait=False, delay_sec=delay)
if code == 0 and wait:
    start = time.monotonic()
    code = APIState.SUCTION_CUP_TOUT
    if delay is not None and delay > 0:
        timeout += delay
    while time.monotonic() - start < timeout:
        ret = GLOBAL.XArm.xarm.get_suction_cup()
        if ret[0] == XCONF.UxbusState.ERR_CODE:
            code = XCONF.UxbusState.ERR_CODE
            break
        if ret[0] == 0:
            if on and ret[1] == 1:
                code = 0
                break
            if not on and ret[1] == 0:
                code = 0
                break
        if not GLOBAL.XArm.xarm_connected or GLOBAL.XArm.xarm.state == 4:
            code = APIState.EMERGENCY_STOP
            break
        yield gen.sleep(0.2)
response(client, cmd_id, code)
```

### Front end

```javascript
self.setSuctionCup = (on, wait, callback, delay) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    on: on,
    wait: wait,
    delay: delay,
  });
  self.sendCmd(window.GlobalConstant.SET_SUCTION_CUP, params, (dict) => {
    // self.codeHandle(dict, 'set suction cup', true);
    if (callback) {
      callback(dict);
    }
  });
};
```

## 17.app\_create\_file

Description:\
Creat Blockly file

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="197">Request Parameter</th><th width="90">Type</th><th width="144">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>userId</td><td>String</td><td>No</td><td>test by default</td></tr><tr><td>version</td><td>String</td><td>No</td><td>Lite6 by default</td></tr><tr><td>folderName</td><td>String</td><td>No</td><td>Name of Blockly</td></tr><tr><td>xmlData</td><td>String</td><td>Yes</td><td>xml data</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>-32:Written failed.</td></tr></tbody></table>

### Background

```python
user_id = data.get('userId', 'test')
xarm_version = data.get('version', GLOBAL.XArm.xarm_type)
root_path = os.path.join(projects_path, user_id, xarm_version, 'app', 'myapp')
file_name = data.get('folderName')
xml_data = data.get('xmlData')
folder_path = os.path.join(root_path, file_name)
code, _ = create_folder(root_path, folder_path)
code, _ = create_file(root_path, file_name, xml_data, xarm_version)
response(client, cmd_id, code, _)
```

### Front end

```javascript
self.appCreateFile = (obj, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, obj);
  self.sendCmd(window.GlobalConstant.APP_CREATE_FILE, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
};
```

## 18.app\_save\_file

Description:\
Save/delete the Blockly project\\

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="198">Request Parameter</th><th width="84">Type</th><th width="144">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>userId</td><td>String</td><td>No</td><td>test by default</td></tr><tr><td>version</td><td>String</td><td>No</td><td>Lite6 by default</td></tr><tr><td>config</td><td>Json</td><td>No</td><td>Directory information. for example：{'selectFilePath':'/4545','expendKeys'['/[D]5125'],'lastAccessTime':1677486596.026057,<br>'deviceType':'xarm6','name':'myapp'}</td></tr><tr><td>children</td><td></td><td></td><td>All files:<br>[{'type':'dir','name':'12312',<br>'children': [{'type':'file','name':'342445','children': [],'uuid': '/342445'}],'uuid': '/[D]12312'}]<br>(name: file name，children: project，type: file type)</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success<br>-32:Written failed.</td></tr></tbody></table>

### Background

```python
user_id = data.get('userId', 'test')
xarm_version = data.get('version', GLOBAL.XArm.xarm_type)
root_path = os.path.join(projects_path, user_id, xarm_version, 'app', 'myapp')
code, _ = save_app_config(root_path, data, xarm_version)
response(client, cmd_id, code, _)
```

### Front end

```javascript
self.saveInfo = (selectCurPath, callback) => {
  if (selectCurPath !== undefined) {
    self.curProjTree.config.selectFilePath = selectCurPath;
  }
  self.curProjTree.config.expendKeys = self.curProjExpandedKeys;
  const expendKeys = [];
  if (self.curProjTree.children) {
    for (let i = 0; i < self.curProjTree.children.length; i++) {
      if (self.curProjExpandedKeys.includes(self.curProjTree.children[i].uuid)) {
        expendKeys.push(self.curProjTree.children[i].uuid);
      }
    }
  }
  self.curProjExpandedKeys = expendKeys;
  self.curProjTree.config.expendKeys = expendKeys;
  const params = self.curProjTree;
  self.sendCmd(window.GlobalConstant.APP_SAVE_INFO, params, (dict) => {
    if (dict.code === 0) {
      if (callback !== undefined && callback !== null) {
        callback();
      }
    };
  });
};
```

## 19.get\_app\_info

Description:\
Get Blockly project directory information\\

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="197">Request Parameter</th><th width="91">Type</th><th width="146">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>userId</td><td>String</td><td>No</td><td>test by default</td></tr><tr><td>version</td><td>String</td><td>No</td><td>Lite6 by default</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success;<br>-31: read failed;<br>-12: not found;<br>-32: written failed;<br>-22: read file directory error</td></tr><tr><td>data</td><td>Json</td><td>Yes</td><td>directory infomation</td></tr></tbody></table>

### Background

```python
user_id = data.get('userId', 'test')
xarm_version = data.get('version', GLOBAL.XArm.xarm_type)
root_path = os.path.join(projects_path, user_id, xarm_version, 'app', 'myapp')
if not os.path.exists(root_path):
    os.makedirs(root_path)
code, result_dir = get_app_config(root_path, xarm_version)
select_file_path = result_dir['config']['selectFilePath']
if not os.path.exists(os.path.join(root_path, select_file_path.lstrip('/'))):
    result_dir['config']['selectFilePath'] = '/untitled'
    uf_app_config = result_dir['children'][0]['children']
    for item in uf_app_config:
        if os.path.exists(os.path.join(root_path, item['name'])):
            result_dir['config']['selectFilePath'] = item['uuid']
            break
response(client, cmd_id, code, data=result_dir)
```

### Front end

```javascript
self.getAppInfo = (callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  // Object.assign(params.data, {
  //   folderName: folderName,
  // });
  self.sendCmd(window.GlobalConstant.APP_GET_INFO, params, (dict) => {
    if (callback) {
	 callback(dict);
    }
  });
};
```

## 20.get\_app\_xml\_data

Description:\
Get the xml Data of Blockly project\\

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="204">Request Parameter</th><th width="97">Type</th><th width="157">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>userId</td><td>String</td><td>No</td><td>test by default</td></tr><tr><td>version</td><td>String</td><td>No</td><td>Lite6 by default</td></tr><tr><td>appName</td><td>String</td><td>Yes</td><td>Name of Blockly project</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0: success;<br>-31: read failed;<br>-12: not found;<br>-22: read file directory error</td></tr><tr><td>xmlData</td><td>Json</td><td>Yes</td><td>xml data</td></tr></tbody></table>

### Background

```python
user_id = data.get('userId', 'test')
xarm_version = data.get('version', GLOBAL.XArm.xarm_type)
root_path = os.path.join(projects_path, user_id, xarm_version, 'app', 'myapp')
app_name = data.get('appName', '')
parent_path = convert_path(app_name)
prj_path = os.path.join(root_path, parent_path)
if prj_path.endswith('app.xml'):
    prj_path = prj_path.rstrip('app.xml')
if not os.path.exists(prj_path) or app_name == '/' or app_name == '':
    return response(client, id, 0, {'xmlData': ''})

if app_name is None:
    code, result_dir = get_app_config(root_path, xarm_version)
    last_app_name = result_dir['config']['selectFilePath'].lstrip('/')
    app_is_exist = os.path.exists(os.path.join(root_path, last_app_name))
    if not app_is_exist:
        uf_app_config = result_dir['children'][0]['children']
        for item in uf_app_config:
            if os.path.exists(os.path.join(root_path, item['name'])):
                last_app_name = item['uuid'].lstrip('/')
                app_is_exist = True
                break
    if app_is_exist:
        parent_path = convert_path(last_app_name)
        prj_path = os.path.join(root_path, parent_path)
        if prj_path.endswith('app.xml'):
            prj_path = prj_path.rstrip('app.xml')
file_path = os.path.join(prj_path, 'app.xml')
_, xml_data = read(file_path)
if os.path.exists(prj_path):
    try:
        with open(os.path.join(prj_path, '.app'), 'w', encoding='utf-8') as f:
            json.dump({
                'lastAccessTime': time.time(),
                'deviceType': xarm_version,
            }, f, sort_keys=True, indent=4, skipkeys=True, separators=(',', ':'), ensure_ascii=False)
    except:
        pass
return response(client, id, _, {'xmlData': xml_data})
```

### Front end

```javascript
self.getAppXmlData = data => new Promise((resolve, reject) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, {
    category: data.category, // 'myapp',
    appName: data.name,
  });
  self.sendCmd('get_app_xml_data', params, (dict) => {
    if (dict.code === 0) {
      resolve(dict.data);
    }
    else {
      if (dict.code !== 10086) reject(dict);
    }
  })
})
```

## 21.xarm\_set\_motion\_config

Description:\
Save motion related parameters\
Button: \[Settings] - \[Motion]

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="202">Request Parameter</th><th width="84">Type</th><th width="143">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>tcpAcc</td><td>int</td><td>No</td><td>Line Motion Acceleration(1-50000)</td></tr><tr><td>jointAcc</td><td>int</td><td>No</td><td>Joint Motion Acceleration(1-1146)</td></tr><tr><td>posStep</td><td>Float</td><td>No</td><td>Line Motion Position Step(0.1-100)</td></tr><tr><td>attitudeStep</td><td>Float</td><td>No</td><td>Line Motion Attitude Step(0.1-100)</td></tr><tr><td>jointStep</td><td>Float</td><td>No</td><td>Joint Motioin Step(0.1-20)</td></tr><tr><td>collSens</td><td>int</td><td>No</td><td>Collision Sensitivity(0-5)</td></tr><tr><td>teachSens</td><td>int</td><td>No</td><td>Teach Sensitivity(1-5)</td></tr><tr><td>initPos</td><td>Array</td><td>No</td><td>Initial（[0,0,0,0,0,0]）</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0 by default</td></tr></tbody></table>

### Background

```python
tcp_acc = int(data.get('tcpAcc', 0))
joint_acc = int(data.get('jointAcc', 0))
if tcp_acc:
    self._xarm_set_params(**{
        'Q': tcp_acc,
    }, is_radian=False)
if joint_acc:
    self._xarm_set_params(**{
        'Q2': joint_acc,
    }, is_radian=False)
GLOBAL.XArm.xarm_params = self._xarm_get_params(is_radian=False)
print(GLOBAL.XArm.xarm_params)
pos_step = float(data.get('posStep', 0))
attitude_step = float(data.get('attitudeStep', 0))
joint_step = float(data.get('jointStep', 0))
if pos_step:
    GLOBAL.XArm.xarm_position_step = pos_step
if attitude_step:
    GLOBAL.XArm.xarm_attitude_step = attitude_step
if joint_step:
    GLOBAL.XArm.xarm_joint_step = joint_step
if tcp_acc:
    GLOBAL.XArm.xarm_tcp_acc = tcp_acc
if joint_acc:
    GLOBAL.XArm.xarm_joint_acc = joint_acc
GLOBAL.XArm.save()
coll_sens = int(data.get('collSens', -1))
teach_sens = int(data.get('teachSens', -1))
sens_is_change = False
if GLOBAL.XArm.xarm.collision_sensitivity != coll_sens and coll_sens >= 0:
    sens_is_change = True
    GLOBAL.XArm.xarm.set_collision_sensitivity(coll_sens)
if GLOBAL.XArm.xarm.teach_sensitivity != teach_sens and teach_sens >= 0:
    sens_is_change = True
    GLOBAL.XArm.xarm.set_teach_sensitivity(teach_sens)
if sens_is_change:
    GLOBAL.XArm.xarm.save_conf()
    GLOBAL.XArm.xarm.set_state(0)
init_pos = data.get('initPos', None)
if init_pos:
    GLOBAL.XArm.xarm_initial_point = list(map(float, init_pos))
GLOBAL.XArm.save()
response(client, cmd_id, 0)
```

### Front end

```javascript
self.setMotionConfig = (config, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, config);
  self.sendCmd(window.GlobalConstant.SET_MOTION_CONFIG, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
};
```

## 22.xarm\_set\_effector\_modbus\_rtu\_cmd

Description:\
Send modbus RTU command\\

```
URL: /ws 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="192">Request Parameter</th><th width="78">Type</th><th width="137">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>isloop</td><td>bool</td><td>No</td><td>whether to send modbus commands cyclically.<br>Yes: True<br>No: False, by default</td></tr><tr><td>mdb_info</td><td>Array</td><td>Yes</td><td>command information. For example:<br>[{'checked': True, 'note': {'cn':'','en': ''},'delay':'','cmd':'00 01 00 02'}]<br>('checked'- whether check the format is correct or not；'note'-{'cn':'','en': ''}'delay'-delay time'cmd'-modbus command)</td></tr><tr><td>end_fmv</td><td>String</td><td>Yes</td><td>The firmware version of end IO board, e.g.‘2.5.9’</td></tr><tr><td>buadrate</td><td>int</td><td>Yes</td><td>modbus baud rate(4800-2500000)</td></tr><tr><td>is_stop</td><td>int</td><td>No</td><td>wheter to stop sending the command</td></tr><tr><td>host_id</td><td>int</td><td>Yes</td><td>Host ID.<br>control box modbus: 11<br>robot arm modbus:9</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>status code</td><td>int</td><td>Yes</td><td>0 by default;<br>1:Failed</td></tr><tr><td>data</td><td>Json</td><td>No</td><td>{'recv': recv,'send':cmd,'send_time':send_time,'recv_time':recv_time}</td></tr></tbody></table>

### Background

```python
if info_dic.get('checked', False):
    try:
        response(client, cmd_id, code=100)
        if cmd and is_run:
            cmd_li = re.sub(',', ' ', cmd)
            cmd_li = re.sub(' +', ' ', cmd_li)
            cmd_li = cmd_li.strip().split(' ')
            int_li = [int(da, 16) for da in cmd_li]
            send_time = ''.join(str(datetime.now())[11:23])
            host_id = data.get('host_id', 9)
            code, ret = GLOBAL.XArm.xarm.getset_tgpio_modbus_data(int_li, host_id=host_id)
            recv_time = ''.join(str(datetime.now())[11:23])
            if code == 0 and ret:
                recv = ''
                for da in ret:
                    re1 = hex(da)[2:]
                    if len(re1) == 1:
                        re1 = '0' + re1
                    recv = recv + ' ' + re1
                    recv = recv.upper()
                response(client, cmd_id, code,
                         {'recv': recv, 'send': cmd, 'send_time': send_time, 'recv_time': recv_time})
            else:
                return response(client, cmd_id, 1, {'send': cmd, })
    except Exception as e:
        logger.error(e)
        return response(client, cmd_id, 1, {'send': cmd, })
```

### Front end

```javascript
self.setEffectorModbusCmd = (data, callback, host_id) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, data, {
    host_id: host_id,
  });
  self.sendCmd(window.GlobalConstant.SET_MODBUS_EFFECTOR_RTU_CMD, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
} 
```

## Upload or Download file (http://{ip}:{port}/project)

## 1.Download Blockly project

Description:\
Download Blockly project

```
URL: /project/download 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="197">Request Parameter</th><th width="82">Type</th><th width="164">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>path</td><td>String</td><td>Yes</td><td>Relative path</td></tr><tr><td>data</td><td>Json</td><td>Yes</td><td>Path information:<br>{"type":"file","name":"Lite6_Logic_test_1","children":[],"uuid":"/Lite6_Logic_test_1"}</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>/</td><td></td><td></td><td></td></tr></tbody></table>

### Background

```python
path = config_path
if not os.path.isfile(path):
    return
filename = path.split('/')[-1]
self.set_header("Accept-Ranges", "bytes")
self.set_etag_header()
self.set_header("Content-Type", 'text/plain')
self.set_header("Access-Control-Allow-Origin", "*")
self.set_header("Access-Control-Allow-Headers", "x-requested-with")
self.set_header('Access-Control-Allow-Methods', 'GET')
request_range = None
range_header = self.request.headers.get("Range")
if range_header:
    request_range = httputil._parse_request_range(range_header)
size = os.stat(path)[stat.ST_SIZE]
if request_range:
    start, end = request_range
    if (start is not None and start >= size) or end == 0:
        self.set_status(416)  # Range Not Satisfiable
        self.set_header("Content-Type", "text/plain")
        self.set_header("Content-Range", "bytes */%s" % (size,))
        return
    if start is not None and start < 0:
        start += size
    if end is not None and end > size:
        end = size
    if size != (end or size) - (start or 0):
        self.set_status(206)  # Partial Content
        self.set_header("Content-Range",
                        httputil._get_content_range(start, end, size))
else:
    start = end = None

if start is not None and end is not None:
    content_length = end - start
elif end is not None:
    content_length = end
elif start is not None:
    content_length = size - start
else:
    content_length = size
self.set_header("Content-Length", content_length)
self.set_header('Content-Disposition', 'attachment; filename=%s' % filename)
content = self.get_content(path, start, end)
if isinstance(content, bytes):
    content = [content]
for chunk in content:
    try:
        self.write(chunk)
        yield self.flush()
    except iostream.StreamClosedError:
        return
```

### Front end

```javascript
 download(name) {
      const data = {};
      if (name === undefined) {
        data.uuid = '/paint';
        data.name = 'all-projects';
      }
      else {
        data.uuid = '/paint/' + name;
        data.name = name;
      }
      let url = `http://${window.GlobalUtil.socketInfo.host}/project/download?path=${window.GlobalConstant.COMMON_PARAMS.userId}/${window.GlobalConstant.COMMON_PARAMS.version}${data.uuid}`
      let full_name = data.name + '.tar.gz';
      fetch(url)
        .then((response) => {
          response.blob().then((blob) => {
            const a = document.createElement('a');
            const url = window.URL.createObjectURL(blob);
            const filename = 'paint-' + full_name;
            a.href = url;
            a.download = filename;
            a.click();
            window.URL.revokeObjectURL(url);
            this.$message({
              message: `${this.$t('downloadSuccess')}: ${filename}`,
              type: 'success',
              duration: 0,
              showClose: true,
            });
          }).catch((e) => {
            this.$message.error(`${this.$t('downloadFailed')}: ${e}`);
          })
        })
        .catch((e) => {
          this.$message.error(`${this.$t('downloadFailed')}: ${e}`);
        })
    }
```

## 2.Import Blockly project

Description:\
Import Blockly project

```
URL: /project/download 
Request Type：	Application/json 
Return Type：	*/* 
```

<table><thead><tr><th width="199">Request Parameter</th><th width="87">Type</th><th width="146">Must be filled</th><th>Description</th></tr></thead><tbody><tr><td>path</td><td>String</td><td>Yes</td><td>Path to upload to the Blockly folder</td></tr><tr><td>data</td><td>Json</td><td>Yes</td><td>Blockly information：<br>{"type":"dir","name":"test_1","uuid":"/[D]test_1"}</td></tr><tr><td>File</td><td>Binary</td><td>Yes</td><td>Binary data of file</td></tr><tr><td>Return Parameter</td><td>Type</td><td>Must be filled</td><td>Description</td></tr><tr><td>result</td><td>String</td><td>Yes</td><td>OK: success;<br>Invalid Args：Invalid parameter</td></tr><tr><td>success</td><td>int</td><td>Yes</td><td>status code.<br>>1: success<br>0: import error<br>-3: the file is empty<br>-1: the type is error</td></tr></tbody></table>

### Background

```python
self.set_header("Access-Control-Allow-Origin", "*")
self.set_header("Access-Control-Allow-Headers", "x-requested-with")
self.set_header('Access-Control-Allow-Methods', 'POST')
path = self.request.arguments.get('path')[0].decode()

path = os.path.join(projects_path, path)
file_metas = self.request.files.get('file', None)  # 提取表单中'name'为'file'的文件元数据
if file_metas:
    success = 0
    for meta in file_metas:
        filename = meta['filename']

        if filename.endswith(('.gz', '.xml')):
            file_path = os.path.join(upload_temp_path, filename.strip())
            if not os.path.exists(upload_temp_path):
                os.makedirs(upload_temp_path)
            with open(file_path, 'wb') as up:
                up.write(meta['body'])
            if not os.path.getsize(file_path):
                return -3
            if filename.endswith('.gz'):
                name = filename.split('.')[0]
                if name.startswith('blockly-'):
                    name = name.split('blockly-')[-1]
                name = name if name else 'tmp'
                extract_path = os.path.join(upload_temp_path, name.strip())
                shutil.unpack_archive(file_path, extract_path, 'gztar')
                blockly_traj_path = os.path.join(extract_path, 'traj')
                if os.path.exists(blockly_traj_path):
                    yield GLOBAL.Core.command.xarm_upload_traj(
                        None, 0, {'path': extract_path, 'is_del_upload_path': False})

                return update_config(path, extract_path, select_data, name)
        else:
            if select_data and isinstance(select_data, dict):
                select_uuid = select_data.get('uuid')
                if select_uuid == '/[D]UF_APPS':
                    select_uuid = '/[D]USER_APPS'
            else:
                select_uuid = '/[D]USER_APPS'

            name = filename.split('.')[0]
            cnt = 0
            new_name = name
            if new_name.startswith('[UF]'):
                new_name = 'UF{}'.format(name[4:])
            while os.path.exists(os.path.join(path, new_name)):
                cnt += 1
                new_name = '{}_{}'.format(name, cnt)
            target_path = os.path.join(path, new_name)
            os.makedirs(target_path)
            shutil.copy(file_path, os.path.join(target_path, 'app.xml'))
            success += 1
            if not os.path.exists(os.path.join(target_path, '.app')):
                with open(os.path.join(target_path, '.app'), 'w', encoding='utf-8') as f:
                    json.dump({
                        'lastAccessTime': 0,
                        'deviceType': GLOBAL.XArm.xarm_type,
                    }, f, sort_keys=True, indent=4, skipkeys=True, separators=(',', ':'),
                        ensure_ascii=False)

            app_config_path = os.path.join(path, '.app_config')
            if os.path.exists(app_config_path):
                with open(app_config_path, 'r', encoding='utf-8') as f:
                    data = json.load(f)
                children = data.get('children', [])
                for child in children:
                    if child['type'] == 'dir' and child['uuid'] == select_uuid:
                        child['children'].append({
                            'uuid': '/' + new_name,
                            'name': new_name,
                            'type': 'file',
                            'children': [],
                        })
                        break

                with open(app_config_path, 'w', encoding='utf-8') as f:
                    json.dump(data, f, sort_keys=True, indent=4, skipkeys=True,
                              separators=(',', ':'), ensure_ascii=False)

        shutil.rmtree(upload_temp_path)
        os.makedirs(upload_temp_path)
    self.write(json.dumps({'result': 'ok', 'success': success}))
else:
    self.write(json.dumps({'result': 'Invalid Args', 'success': -2}))
```

### Front end

```javascript
 <el-upload :disabled="isRunning" multiple class="app-uploader com-edit-btn" :data="uploadData" :action="`http://${window.GlobalUtil.socketInfo.host}/project/upload`"
                    :show-file-list="false" :on-success="handleUploadProjectSuccess" :before-upload="beforeUploadProject">
                    <img :disabled="isRunning" @click="onUpload" src="../assets/img/blockly/upload-icon2.svg" />
                  </el-upload>
```

## Status report message (http://{ip}:{port}/project)

1.xarm\_tcp\_pose\
2.xarm\_joint\_pose\
3.xarm\_error, xarm\_warn\
4.xarm\_mode\\

### Front end

```javascript
// Connect websocket to get push
self.init_onmessage = (onmessage) => {
  if(onmessage.cmd === 'devices_status_report'){
// so something
}
};
```
