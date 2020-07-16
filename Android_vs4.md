# UI Thread Update
```java
private Handler mHandler = new Handler(Looper.getMainLooper()/*MainThread의 Loop*/);
private int mPercentage;
```
```java
 mThread = new Thread(new Runnable() {
            @Override
            public void run() {
                mButton.setText("Download");
                //WorkerThread가 UI에 관여하는게 아니라 UI
                mProgressBar.setProgress(0);
                for (int i = 0; i <= 100; i++) {
                    try { Thread.sleep(100); } catch (InterruptedException e) { e.printStackTrace(); }


                    //WorkerThread의 코드 중 일부를 
                    // MainThread와 연결되어 있는 UI Thread로 보내어 직접 수행하게 만든다.
                    mPercentage = i;
                    mHandler.post(new Runnable() {//실제 UI에서 실행할 코드를 가져온다. 즉 여기는 UITHREAD
                        @Override
                        public void run() {
                            //UI를 대신 UPDATE
                            mButton.setText(mPercentage + "%");
                            mProgressBar.setProgress(mPercentage);
                        }
                    });
                }
                mThread= null;
            }
        });
        mThread.start();
```
Android에서 간단하게 `UI Thread`에 한해서 핸들러 없이 바로 메세지 큐에`Runnable` 객체를 보내는 메서드를 제공한다.

`RunOnUiThread`를 제공
```java
 runOnUiThread(new Runnable() {//실제 UI에서 실행할 코드를 가져온다. 즉 여기는 UITHREAD
    @Override
    public void run() {
        //UI를 대신 UPDATE
        mButton.setText(mPercentage + "%");
        mProgressBar.setProgress(mPercentage);
    }
});
```



