## Async Task, Threads, Pools, Executors

#### Basic Thread
Basic Thread example

long rootRandom = System.currentTimeMillis();

    private  class RandomThread extends Thread {
        
            long seed;
            RandomThread (long seed) {
                this.seed = seed;
            }

            @Override
            public void run() {
                Random seededRandom = new Random(seed);
                rootRandom = seededRandom.nextInt();
            }
    }