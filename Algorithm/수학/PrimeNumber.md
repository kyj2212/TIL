


# 에라토스테네스의 체

Prime Num 구하기

####  O(N^(1/2) 가능

### TreeSet vs. PriorityQueue vs. Collections.sort(ArrayList)
TreeSet > PriorityQueue > Collections.sort(ArrayList) 순으로 빠름



    public static void main(String[] args) throws IOException {
        System.out.println("[안내] 0을 입력하시면 입력이 종료 됩니다.");
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int i = 1;
        PriorityQueue<Integer> q = new PriorityQueue(Collections.reverseOrder());
        ArrayList<Integer> aa = new ArrayList();
        TreeSet<Integer> t = new TreeSet();

        while(true) {
            System.out.printf("숫자 " + i + " : ");

            try {
                int num = Integer.parseInt(br.readLine());
                if (num == 0) {
                    break;
                }

                if (t.contains(num)) {
                    System.out.println("[입력오류] : 이미 입력된 숫자입니다.");
                } else {
                    t.add(num);
                    q.add(num);
                    aa.add(num);
                    ++i;
                }
            } catch (NumberFormatException var7) {
                System.out.println("[입력오류] : 숫자로 입력해주세요.");
            }
        }

        System.out.println("treeset : " + treeset(t));
        System.out.println("priority : " + priorityarray(q));
        System.out.println("array : " + array(aa));
    }

    static long priorityarray(PriorityQueue<Integer> q) {
        ArrayList<Integer> primelist = new ArrayList();
        long start = System.currentTimeMillis();
        boolean[] isPrime = noPrime((Integer)q.peek());
        Iterator var5 = q.iterator();

        while(var5.hasNext()) {
            int i = (Integer)var5.next();
            if (!isPrime[i]) {
                primelist.add(i);
            }
        }

        System.out.printf("결과 : " + primelist);
        long end = System.currentTimeMillis();
        return end - start;
    }

    static long array(ArrayList<Integer> aa) {
        Collections.sort(aa);
        ArrayList<Integer> primelist = new ArrayList();
        long start = System.currentTimeMillis();
        if (!aa.isEmpty()) {
            boolean[] np = noPrime((Integer)aa.get(aa.size() - 1));
            Iterator var5 = aa.iterator();

            while(var5.hasNext()) {
                int j = (Integer)var5.next();
                if (!np[j]) {
                    primelist.add(j);
                }
            }

            System.out.printf("결과 : " + primelist);
        } else {
            System.out.println("[출력오류] : 값을 입력하세요.");
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    static long treeset(TreeSet<Integer> t) {
        ArrayList<Integer> primelist = new ArrayList();
        long start = System.currentTimeMillis();
        if (!t.isEmpty()) {
            boolean[] np = noPrime((Integer)t.pollLast());
            Iterator var5 = t.iterator();

            while(var5.hasNext()) {
                int j = (Integer)var5.next();
                if (!np[j]) {
                    primelist.add(j);
                }
            }

            System.out.printf("결과 : " + primelist);
        } else {
            System.out.println("[출력오류] : 값을 입력하세요.");
        }

        long end = System.currentTimeMillis();
        return end - start;
    }

    static boolean[] noPrime(int N) {
        boolean[] prime = new boolean[N + 1];
        prime[0] = prime[1] = false;

        for(int i = 2; i * i <= N; ++i) {
            if (!prime[i]) {
                for(int j = i * i; j <= N; j += i) {
                    prime[j] = true;
                }
            }
        }

        return prime;
    }
