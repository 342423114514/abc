local Translations = {
    ["Main"] = "主线",
    ["Stealer"] = "功能 1",
    ["Helper"] = "功能 2",
    ["Player"] = "玩家类",
    ["Finder"] = "查找器",
    ["Server"] = "服务器类",
    ["Discord"] = "Q群和红辣椒dc群",
    ["Select Item to Buy"] = "选择自动买的东西",
    ["Auto Buy Item"] = "自动购买",
    ["Min money per sec to buy"] = "脑红最小金额",
    ["Auto buy Brainrot"] = "自动购买",
    ["Anti AFK"] = "发 AFK",
    ["Auto Lock Base"] = "自动关门",
    ["Auto Collect"] = "自动收钱",
    ["Collect Delay (minutes)"] = "收钱间隔",
    ["Float (Need 7 rebirths, C keybind)"] = "假人飞天",
    ["Laser Steal (Auto user Laser Cape when steal)"] = "激光窃取",
    ["Steal Upstairs(Y keybind, turn off chilli speed boost to use)"] = "偷楼上",
    ["Auto Steal"] = "自动窃取",
    ["Display Auto Steal on screen"] = "自动窃取快捷键",
    ["Auto Steal Nearest Brainrot"] = "偷最近的脑红",
    ["Auto kick"] = "偷完自动退",
    ["Aimbot"] = "目标",
    ["Highest Value Brainrot ESP"] = "看最贵脑红",
    ["Timer ESP"] = "看开门时间",
    ["Player ESP"] = "透视",
    ["Anti Ragdoll"] = "反布偶",
    ["Speed boost (3 rebirth require, Q keybind for PC"] = "速度增强(偷时无用)",
    ["Infinity Jump"] = "无限跳跃",
    ["Chilli Booster"] = "红辣椒助推器",
    ["Auto Load Script"] = "自动加载",
    ["Start server hop to find target"] = "开始寻找",
    ["Webhook URL"] = "Webhook URL",
    ["Find High Value Brainrot Server"] = "找高价值服务器",
    ["Min money per sec to find"] = "最小金额",
    ["Find < 2 min Guaranteed Mythic Server"] = "查找时间小于2分钟",
    ["Server Hop"] = "进入服务器",
    ["Job-ID Input"] = "输入服务器ID",
    ["Join Job-ID"] = "加入服务器ID",
    ["Copy Job-ID"] = "复制服务ID",
    ["Reduce Graphics"] = "减少图形",
    ["Works best on low-ping servers (<120)"] = "延迟越低越好<120",
    ["Join my Discord to join secret servers 100% within 1 min"] = "汉化作者：朝霞Q群1038849170",
    ["Discord Link (Click to copy)"] = "加入红辣椒DC群",
    ["Secret giveaway happening now on my Discord"] = "翻译不准确的请见谅",
    ["Rejoin & Auto Load Script"] = "重新加入时自动加载脚本",
    ["Chilli Booster"] = "红辣椒助推器",
    ["Speed Boost"] = "速度提升",
    ["Jump Boost"] = "跳跃提升",
    ["Gravity"] = "重力",
    ["Chilli Speed Boost"] = "红辣椒速度增强(无用)",
    ["Steal a Brainot-Chilli Hub"] = "偷走脑红-红辣椒(汉化者：朝霞",
    ["Chilli Float"] = "假人飞天",
    ["Laser Steal"] = "激光窃取",
    ["Steal upstairs"] = "偷楼上",
    ["OFF"] = "关闭",
    ["ONN"] = "开启",
}

local function translateText(text)
    if not text or type(text) ~= "string" then return text end
    
    if Translations[text] then
        return Translations[text]
    end
    
    for en, cn in pairs(Translations) do
        if text:find(en) then
            return text:gsub(en, cn)
        end
    end
    
    return text
end

local function setupTranslationEngine()
    local success, err = pcall(function()
        local oldIndex = getrawmetatable(game).__newindex
        setreadonly(getrawmetatable(game), false)
        
        getrawmetatable(game).__newindex = newcclosure(function(t, k, v)
            if (t:IsA("TextLabel") or t:IsA("TextButton") or t:IsA("TextBox")) and k == "Text" then
                v = translateText(tostring(v))
            end
            return oldIndex(t, k, v)
        end)
        
        setreadonly(getrawmetatable(game), true)
    end)
    
    if not success then
        warn("元表劫持失败:", err)
       
        local translated = {}
        local function scanAndTranslate()
            for _, gui in ipairs(game:GetService("CoreGui"):GetDescendants()) do
                if (gui:IsA("TextLabel") or gui:IsA("TextButton") or gui:IsA("TextBox")) and not translated[gui] then
                    pcall(function()
                        local text = gui.Text
                        if text and text ~= "" then
                            local translatedText = translateText(text)
                            if translatedText ~= text then
                                gui.Text = translatedText
                                translated[gui] = true
                            end
                        end
                    end)
                end
            end
            
            local player = game:GetService("Players").LocalPlayer
            if player and player:FindFirstChild("PlayerGui") then
                for _, gui in ipairs(player.PlayerGui:GetDescendants()) do
                    if (gui:IsA("TextLabel") or gui:IsA("TextButton") or gui:IsA("TextBox")) and not translated[gui] then
                        pcall(function()
                            local text = gui.Text
                            if text and text ~= "" then
                                local translatedText = translateText(text)
                                if translatedText ~= text then
                                    gui.Text = translatedText
                                    translated[gui] = true
                                end
                            end
                        end)
                    end
                end
            end
        end
        
        local function setupDescendantListener(parent)
            parent.DescendantAdded:Connect(function(descendant)
                if descendant:IsA("TextLabel") or descendant:IsA("TextButton") or descendant:IsA("TextBox") then
                    task.wait(0.1)
                    pcall(function()
                        local text = descendant.Text
                        if text and text ~= "" then
                            local translatedText = translateText(text)
                            if translatedText ~= text then
                                descendant.Text = translatedText
                            end
                        end
                    end)
                end
            end)
        end
        
        pcall(setupDescendantListener, game:GetService("CoreGui"))
        local player = game:GetService("Players").LocalPlayer
        if player and player:FindFirstChild("PlayerGui") then
            pcall(setupDescendantListener, player.PlayerGui)
        end
        
        while true do
            scanAndTranslate()
            task.wait(3)
        end
    end
end

task.wait(2)

setupTranslationEngine()

local success, err = pcall(function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/tienkhanh1/spicy/main/Chilli.lua"))()



end)

if not success then
    warn("加载失败:", err)
end
