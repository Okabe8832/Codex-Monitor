# Codex-Monitor
Get a quick overview of the Codex's operating status via Mac system audio cues.
通过MAC系统声音提示简单了解codex工作状态


于codex任务要求添加：
执行任务过程中，请每完成一个关键步骤，或至少每 1 分钟，向本地文件 ~/Desktop/codex_alive.txt 追加一行进度。
格式为：当前时间 + 当前进度。
任务完成后，最后追加一行 DONE。











terminal运行：

WATCH="$HOME/Desktop/codex_alive.txt"; STAMP="/tmp/codex_alive_seen"; touch "$WATCH" "$STAMP"; tick=0; while true; do
  tick=$((tick+1))

  if [ $((tick % 30)) -eq 1 ]; then
    echo "awake heartbeat: $(date '+%F %T')"
    afplay /System/Library/Sounds/Ping.aiff
  fi

  if [ "$WATCH" -nt "$STAMP" ]; then
    echo "codex progress: $(date '+%F %T')"
    tail -1 "$WATCH"
    afplay /System/Library/Sounds/Tink.aiff
    touch "$STAMP"
  fi

  sleep 2
done
