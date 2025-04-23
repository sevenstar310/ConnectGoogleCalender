const CALENDAR_IN_OUTPUT = "";
const CALENDAR_ID_OUTPUT = "";


// 同期元カレンダーのイベントをチェックし、同期先に未登録のものだけをコピー
function syncCalendars() {
  const now = new Date();
  // 取得範囲: 過去7日〜未来30日
  const startTime = new Date(now.getTime() - 7 * 24 * 60 * 60 * 1000);
  const endTime   = new Date(now.getTime() + 30 * 24 * 60 * 60 * 1000);

  const sourceCal = CalendarApp.getCalendarById(CALENDAR_IN_OUTPUT);
  const targetCal = CalendarApp.getCalendarById(CALENDAR_ID_OUTPUT);

  if (!sourceCal || !targetCal) {
    Logger.log('同期元 or 同期先カレンダーが見つかりません');
    return;
  }

  // 同期元のイベント一覧を取得
  const sourceEvents = sourceCal.getEvents(startTime, endTime);

  sourceEvents.forEach(event => {
    const title    = "不在";
    const start    = event.getStartTime();
    const end      = event.getEndTime();
    const desc     = event.getDescription();
    const loc      = event.getLocation();
    const isAllDay = event.isAllDayEvent();

    // 同期先に同期間・同タイトルのイベントがあるか判定
    const existing = targetCal.getEvents(start, end)
      .some(e => e.getTitle() === title);

    if (!existing) {
      if (isAllDay) {
        // 終日イベントとして作成
        targetCal.createAllDayEvent(title, start, {
          description: desc,
          location: loc
        });
      } else {
        // 時間指定イベントとして作成
        targetCal.createEvent(title, start, end, {
          description: desc,
          location: loc
        });
      }
      Logger.log(`Created: ${title} [${start.toISOString()} – ${end.toISOString()}]`);
    }
  });
}
