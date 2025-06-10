[English Version](README_en.md)

## Windows -> Linux 優缺點

[Youtube Tutorial - Windows -> Linux 優缺點](https://youtu.be/wCQv7rfv69Q)

從 Windows 切換到 Linux (日常作業 ubuntu 18.04) 也已經 4 個月了,

還有個 VM 環境是 Debian 10 (KDE), 開發專用, Debian 真的穩到爆 :satisfied:

來分享一下從 Windows -> Linux 的優缺點以及使用心得 :smile:

### 先說切換到 Linux 的優點

1. 自由度(客製化)超級高, 尤其是那個 DE, 真的超級好玩 :smile:

你喜歡/不喜歡什麼軟體都可以找其他的來取代, 快捷鍵想怎麼設定就怎麼設定,

有一種完全的掌握系統的感覺, 想幹嘛就幹嘛, 自由度極高。

延伸閱讀 [Linux 桌面環境 Desktop Environment](https://github.com/twtrubiks/linux-note/tree/master/linux-de)

2. Linux 使用資源相對 Windows 少很多, 尤其是在 8GB Ram (又無法擴充的環境下),

儘管我是使用 GNOME 這個比較吃資源的 DE, 使用起來還是比 Windows 輕便且有更多的資源可使用,

如果你有比較舊的電腦（筆電）, 都可以考慮安裝 Linux (安裝比較輕量的 distro)。

延伸閱讀 [認識 Linux 發行版](https://github.com/twtrubiks/linux-note/tree/master/linux-distro)

3. Linux 穩定又可靠, 且不會強迫你要更新系統, 尤其是 Windows 都強迫自動更新, 然後 Linux 更新

除非是很重大的核心更新,不然基本上更新完是不需要重開機的, 也就是說你的電腦完全不用關機,

隨時隨地都可以馬上進入工作, 不會像 Windows 等你回到座位, 你就發現他被重開機了 :sweat:

但為了愛地球, 長時間不使用還是建議關機 :smile:

4. Linux 不管在哪裡都可以安裝, 尤其是在 server 中, 穩定性以及可靠性高絕對是首選 :smirk:

5. Linux 是 free 而且開源的, 也就代表幾乎不需要付費, 也因為是開源的, 可能會比較多人去檢查 code,

也可能會比 Windows(封閉) 相對安全。

6. Linux 不需要裝防毒軟體 (以前 Windows 我也只使用內建的防毒軟體), 但不代表 Linux 不會中毒

(雖然我還沒發生過XD), 而且 Linux 應該比 Windows 安全。

7. 對軟體工程師來說, 很多事情變簡單了 :smile: 有些東西在 Linux 上很簡單的一件事, 在 Windows

上超級無敵霹靂奇怪 :angry: 像是 k8s 的安裝, 又或是 docker 在 Windows 上的安裝(一定要專業版),

有時候真的會讓自己很煩 :disappointed:

8. Linux(Ubuntu) 有 LTS, 也就是說有長期支援, 如果沒特別的需求, 可以穩穩用5年。

9. Linux 上的 terminal (zsh+tmux) 好用非常多

延伸閱讀 [Ubuntu 如何安裝 Oh My ZSH + tmux](https://github.com/twtrubiks/Linux-note/tree/master/zsh-tmux-tutorual)

10. Linux 不用碎片整理, 相信大家在 Windows 上都聽過磁碟重整(碎片整理), 會需要這個是因為

Windows 是使用 NTFS, 而現在大多數的 Linux 都是使用 ext4 (他不需要碎片整理)。

11. 很多好用的工具只有在 Linux 才有 :satisfied:

12. 不用擔心遇到問題找不到答案, 因為 Linux 社群是非常活躍的 :satisfied:

13. Linux 讓電腦變得更有趣了 :laughing: 讓使用電腦變成一種學習

### 切換到 Linux 的缺點

1. 需要中文輸入法的人會覺得在 Linux 上中文輸入法比較不好用 (被 Windows 寵壞了 :sweat_smile:)

延伸閱讀 [Linux 中更像微軟更好用的中文輸入法 hime](https://github.com/twtrubiks/Linux-note/tree/master/hime-tutorial)

2. 玩特定遊戲變得稍微比較麻煩, 通常都要額外的設定 (但對我而言, 換 Linux 就是要戒掉遊戲) :stuck_out_tongue_closed_eyes:

當然也有一些 distro 是比較適合玩遊戲的. 如果你都玩 Steam 上的遊戲, 在 Linux 應該是完全沒問題的 :smile:

3. 如果常常需要使用金融卡, 像是線上 ATM 那類的, 蠻多銀行不支援 Linux , 真的有點麻煩 :persevere:

4. 驅動程式偶爾需要自己另外去抓, 因為 Linux 安裝驅動的方式和 Windows 不太一樣, Windows 會包山包海一次

幫你全裝, 但 LINUX 也已經支援很多了, 像是 linux-firmware 這個 package.

5. 如果你是 Microsoft Office 重度使用者可能會有點麻煩, 在 Linux 上只有 LibreOffice  或線上版可以使用。

6. 這邊應該不算缺點, 算是小小的抱怨 :joy: 遠端機器的部份我使用過 AnyDesk,  TeamViewer,  VNC Server-RealVNC

(都是個人使用), 但 Linux 上如果安裝 VNC Server-RealVNC 似乎會變成要收費? ( Windows 上卻是免費), 但也沒關係,

因為 Linux 上 server 端或 client 端的遠端軟體非常的多, 但多數都有一個限制, 就是必須要在內網裡(或是有固定ip).
