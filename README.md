# HammerSpoon
```lua
-- Greeting when log in
hs.alert.show('Hello, 호영 ☺️')

-- Reload config
hs.hotkey.bind({'option', 'cmd'}, 'r', hs.reload)

-- Binding applications
hs.hotkey.bind({'shift', 'option'}, 'C', function ()
    hs.application.launchOrFocus('Google Chrome')
end)

hs.hotkey.bind({'shift', 'option'}, 'V', function ()
    hs.application.launchOrFocus('Visual Studio Code')
end)

hs.hotkey.bind({'shift', 'option'}, 'T', function ()
    hs.application.launchOrFocus('Terminal')
end)

hs.hotkey.bind({'shift', 'option'}, 'S', function ()
    hs.application.launchOrFocus('Slack')
end)

hs.hotkey.bind({'shift', 'option'}, 'K', function ()
    hs.application.launchOrFocus('KakaoTalk')
end)

hs.hotkey.bind({'shift', 'option'}, 'Z', function ()
    hs.application.launchOrFocus('zoom.us')
end)

hs.hotkey.bind({'shift', 'option'}, 'N', function ()
    hs.application.launchOrFocus('Notion')
end)

hs.hotkey.bind({'shift', 'option'}, 'F', function ()
  hs.application.launchOrFocus('finder')
end)

hs.hotkey.bind({'shift', 'option'}, 'M', function ()
  hs.application.launchOrFocus('microsoft To Do')
end)


-- size up
-- === sizeup ===
--
-- SizeUp emulation for hammerspoon
--
-- To use, you can tweak the key bindings and the margins

local sizeup = { }

--------------
-- Bindings --
--------------

--- Split Screen Actions ---
-- Send Window Left
hs.hotkey.bind({"ctrl","alt","cmd"}, "Left", function()
  sizeup.send_window_left()
end)
-- Send Window Right
hs.hotkey.bind({"ctrl","alt","cmd"}, "Right", function()
  sizeup.send_window_right()
end)
-- Send Window Up
hs.hotkey.bind({"ctrl","alt","cmd"}, "Up", function()
  sizeup.send_window_up()
end)
-- Send Window Down
hs.hotkey.bind({"ctrl","alt","cmd"}, "Down", function()
  sizeup.send_window_down()
end)

--- Quarter Screen Actions ---
-- Send Window Upper Left
hs.hotkey.bind({"ctrl", "alt", "shift"}, "Left", function()
  sizeup.send_window_upper_left()
end)
-- Send Window Upper Right
hs.hotkey.bind({"ctrl","alt", "shift"}, "Up", function()
  sizeup.send_window_upper_right()
end)
-- Send Window Lower Left
hs.hotkey.bind({"ctrl", "alt", "shift"}, "Down", function()
  sizeup.send_window_lower_left()
end)
-- Send Window Lower Right
hs.hotkey.bind({"ctrl", "alt", "shift"}, "Right", function()
  sizeup.send_window_lower_right()
end)

-- Vertical Quarter Screen Actions ---
-- Send Window to 1st quarter
hs.hotkey.bind({"ctrl", "alt", "shift"}, "1", function()
  sizeup.send_window_1st_vertical_quarter()
end)
-- Send Window to 2nd quarter
hs.hotkey.bind({"ctrl","alt", "shift"}, "2", function()
  sizeup.send_window_2nd_vertical_quarter()
end)
-- Send Window to 3rd quarter
hs.hotkey.bind({"ctrl","alt", "shift"}, "3", function()
  sizeup.send_window_3rd_vertical_quarter()
end)
-- Send Window to 4th quarter
hs.hotkey.bind({"ctrl","alt", "shift"}, "4", function()
  sizeup.send_window_4th_vertical_quarter()
end)

--- Multiple Monitor Actions ---
-- Send Window Prev! Monitor
hs.hotkey.bind({ "ctrl", "alt" }, "Left", function()
  sizeup.send_window_next_monitor()
end)
-- Send Window Next Monitor
hs.hotkey.bind({ "ctrl", "alt" }, "Right", function()
  sizeup.send_window_prev_monitor()
end)

--- Spaces Actions ---

-- Apple no longer provides any reliable API access to spaces.
-- As such, this feature no longer works in SizeUp on Yosemite and
-- Hammerspoon currently has no solution that isn't a complete hack.
-- If you have any ideas, please visit the ticket

--- Snapback Action ---
hs.hotkey.bind({"ctrl","alt","cmd"}, "Z", function()
  sizeup.snapback()
end)
--- Other Actions ---
-- Make Window Full Screen
hs.hotkey.bind({"cmd","alt","ctrl"}, "M", function()
  sizeup.maximize()
end)
-- Send Window Center
hs.hotkey.bind({"cmd","alt","ctrl"}, "J", function()
  -- sizeup.move_to_center_absolute({w=800, h=600})
  sizeup.move_to_center_relative({w=0.5, h=1})
end)


-------------------
-- Configuration --
-------------------

-- Margins --
sizeup.screen_edge_margins = {
  top =    0, -- px
  left =   0,
  right =  0,
  bottom = 0
}
sizeup.partition_margins = {
  x = 0, -- px
  y = 0
}

-- Partitions --
sizeup.split_screen_partitions = {
  x = 0.5, -- %
  y = 0.5
}
sizeup.quarter_screen_partitions = {
  x = 0.5, -- %
  y = 0.5
}
sizeup.vertical_quarter_screen_partitions = {
  x = 0.25,
  y = 1,
}


----------------
-- Public API --
----------------

function sizeup.send_window_left()
  local s = sizeup.screen()
  local ssp = sizeup.split_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Left", {
    x = s.x,
    y = s.y,
    w = (s.w * ssp.x) - sizeup.gutter().x,
    h = s.h
  })
end

function sizeup.send_window_right()
  local s = sizeup.screen()
  local ssp = sizeup.split_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Right", {
    x = s.x + (s.w * ssp.x) + g.x,
    y = s.y,
    w = (s.w * (1 - ssp.x)) - g.x,
    h = s.h
  })
end

function sizeup.send_window_up()
  local s = sizeup.screen()
  local ssp = sizeup.split_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Up", {
    x = s.x,
    y = s.y,
    w = s.w,
    h = (s.h * ssp.y) - g.y
  })
end

function sizeup.send_window_down()
  local s = sizeup.screen()
  local ssp = sizeup.split_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Down", {
    x = s.x,
    y = s.y + (s.h * ssp.y) + g.y,
    w = s.w,
    h = (s.h * (1 - ssp.y)) - g.y
  })
end

function sizeup.send_window_1st_vertical_quarter()
  local s = sizeup.screen()
  local vqsp = sizeup.vertical_quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("1st V-quater", {
    x = s.x,
    y = s.y,
    w = (s.w * vqsp.x) - g.x,
    h = s.h
  })
end

function sizeup.send_window_2nd_vertical_quarter()
  local s = sizeup.screen()
  local vqsp = sizeup.vertical_quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("2nd V-quater", {
    x = s.x + (s.w * vqsp.x) + g.x,
    y = s.y,
    w = (s.w * vqsp.x) - g.x,
    h = s.h
  })
end

function sizeup.send_window_3rd_vertical_quarter()
  local s = sizeup.screen()
  local vqsp = sizeup.vertical_quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("3rd V-quater", {
    x = s.x + (s.w * 2 * vqsp.x) + g.x,
    y = s.y,
    w = (s.w * vqsp.x) - g.x,
    h = s.h
  })
end

function sizeup.send_window_4th_vertical_quarter()
  local s = sizeup.screen()
  local vqsp = sizeup.vertical_quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("4th V-quater", {
    x = s.x + (s.w * 3 * vqsp.x) + g.x,
    y = s.y,
    w = (s.w * vqsp.x) - g.x,
    h = s.h
  })
end

function sizeup.send_window_upper_left()
  local s = sizeup.screen()
  local qsp = sizeup.quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Upper Left", {
    x = s.x,
    y = s.y,
    w = (s.w * qsp.x) - g.x,
    h = (s.h * qsp.y) - g.y
  })
end

function sizeup.send_window_upper_right()
  local s = sizeup.screen()
  local qsp = sizeup.quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Upper Right", {
    x = s.x + (s.w * qsp.x) + g.x,
    y = s.y,
    w = (s.w * (1 - qsp.x)) - g.x,
    h = (s.h * (qsp.y)) - g.y
  })
end

function sizeup.send_window_lower_left()
  local s = sizeup.screen()
  local qsp = sizeup.quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Lower Left", {
    x = s.x,
    y = s.y + (s.h * qsp.y) + g.y,
    w = (s.w * qsp.x) - g.x,
    h = (s.h * (1 - qsp.y)) - g.y
  })
end

function sizeup.send_window_lower_right()
  local s = sizeup.screen()
  local qsp = sizeup.quarter_screen_partitions
  local g = sizeup.gutter()
  sizeup.set_frame("Lower Right", {
    x = s.x + (s.w * qsp.x) + g.x,
    y = s.y + (s.h * qsp.y) + g.y,
    w = (s.w * (1 - qsp.x)) - g.x,
    h = (s.h * (1 - qsp.y)) - g.y
  })
end

function sizeup.send_window_prev_monitor()
  hs.alert.show("Prev Monitor")
  local win = hs.window.focusedWindow()
  local nextScreen = win:screen():previous()
  win:moveToScreen(nextScreen)
end

function sizeup.send_window_next_monitor()
  hs.alert.show("Next Monitor")
  local win = hs.window.focusedWindow()
  local nextScreen = win:screen():next()
  win:moveToScreen(nextScreen)
end

-- snapback return the window to its last position. calling snapback twice returns the window to its original position.
-- snapback holds state for each window, and will remember previous state even when focus is changed to another window.
function sizeup.snapback()
  local win = sizeup.win()
  local id = win:id()
  local state = win:frame()
  local prev_state = sizeup.snapback_window_state[id]
  if prev_state then
    win:setFrame(prev_state)
  end
  sizeup.snapback_window_state[id] = state
end

function sizeup.maximize()
  sizeup.set_frame("Full Screen", sizeup.screen())
end

--- move_to_center_relative(size)
--- Method
--- Centers and resizes the window to the the fit on the given portion of the screen.
--- The argument is a size with each key being between 0.0 and 1.0.
--- Example: win:move_to_center_relative(w=0.5, h=0.5) -- window is now centered and is half the width and half the height of screen
function sizeup.move_to_center_relative(unit)
  local s = sizeup.screen()
  sizeup.set_frame("Center", {
    x = s.x + (s.w * ((1 - unit.w) / 2)),
    y = s.y + (s.h * ((1 - unit.h) / 2)),
    w = s.w * unit.w,
    h = s.h * unit.h
  })
end

--- move_to_center_absolute(size)
--- Method
--- Centers and resizes the window to the the fit on the given portion of the screen given in pixels.
--- Example: win:move_to_center_relative(w=800, h=600) -- window is now centered and is 800px wide and 600px high
function sizeup.move_to_center_absolute(unit)
  local s = sizeup.screen()
  sizeup.set_frame("Center", {
    x = (s.w - unit.w) / 2,
    y = (s.h - unit.h) / 2,
    w = unit.w,
    h = unit.h
  })
end


------------------
-- Internal API --
------------------

-- SizeUp uses no animations 
hs.window.animation_duration = 0.0
-- Initialize Snapback state
sizeup.snapback_window_state = { }
-- return currently focused window
function sizeup.win()
  return hs.window.focusedWindow()
end
-- display title, save state and move win to unit
function sizeup.set_frame(title, unit)
  hs.alert.show(title)
  local win = sizeup.win()
  sizeup.snapback_window_state[win:id()] = win:frame()
  return win:setFrame(unit)
end
-- screen is the available rect inside the screen edge margins
function sizeup.screen()
  local screen = sizeup.win():screen():frame()
  local sem = sizeup.screen_edge_margins
  return {
    x = screen.x + sem.left,
    y = screen.y + sem.top,
    w = screen.w - (sem.left + sem.right),
    h = screen.h - (sem.top + sem.bottom)
  }
end
-- gutter is the adjustment required to accomidate partition
-- margins between windows
function sizeup.gutter()
  local pm = sizeup.partition_margins
  return {
    x = pm.x / 2,
    y = pm.y / 2
  }
end

--- hs.window:moveToScreen(screen)
--- Method
--- move window to the the given screen, keeping the relative proportion and position window to the original screen.
--- Example: win:moveToScreen(win:screen():next()) -- move window to next screen
function hs.window:moveToScreen(nextScreen)
  local currentFrame = self:frame()
  local screenFrame = self:screen():frame()
  local nextScreenFrame = nextScreen:frame()
  self:setFrame({
    x = ((((currentFrame.x - screenFrame.x) / screenFrame.w) * nextScreenFrame.w) + nextScreenFrame.x),
    y = ((((currentFrame.y - screenFrame.y) / screenFrame.h) * nextScreenFrame.h) + nextScreenFrame.y),
    h = ((currentFrame.h / screenFrame.h) * nextScreenFrame.h),
    w = ((currentFrame.w / screenFrame.w) * nextScreenFrame.w)
  })
end

-- 화면 테두리를 표시하기 위한 변수 설정
local grayBorder = nil

-- 입력 상태 감지 및 화면 테두리 설정 함수
local function updateGrayBorder()
    local source = hs.keycodes.currentSourceID()
    local focusedScreen = hs.window.focusedWindow() and hs.window.focusedWindow():screen()

    if source == "com.apple.inputmethod.Korean.2SetKorean" and focusedScreen then
        local screenFrame = focusedScreen:fullFrame()
        
        -- 한글 입력 상태이면서 테두리가 없으면 추가합니다
        if not grayBorder then
            grayBorder = hs.drawing.rectangle(hs.geometry.rect(screenFrame.x, screenFrame.y, screenFrame.w, screenFrame.h))
            grayBorder:setStrokeColor({red=0.5, green=0.6, blue=0.0, alpha=0.6})
            grayBorder:setFill(false)  -- 배경을 채우지 않고 테두리만 설정
            grayBorder:setStrokeWidth(30)  -- 테두리 두께 설정
            grayBorder:setLevel(hs.drawing.windowLevels.overlay)
            grayBorder:show()
        else
            -- 이미 테두리가 있으면 현재 화면 위치로 업데이트
            grayBorder:setFrame(hs.geometry.rect(screenFrame.x, screenFrame.y, screenFrame.w, screenFrame.h))
        end
    else
        -- 영문 상태일 때 테두리가 있으면 제거합니다
        if grayBorder then
            grayBorder:delete()
            grayBorder = nil
        end
    end
end

-- 입력 소스 변경 및 활성화된 창 변경 감지 이벤트 설정
hs.keycodes.inputSourceChanged(updateGrayBorder)
hs.window.filter.default:subscribe(hs.window.filter.windowFocused, updateGrayBorder)
```
