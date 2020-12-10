学习笔记

深度优先遍历
    代码简洁。但是在求最小路径的时候容易超时。优先采用广度优先遍历。
   

广度优先遍历
    逻辑比较清晰，符合人脑思维。
    
    广度优先遍历新知识点，双向广度优先遍历额。能采用双向广度优先遍历的前提是
    
知道开始，知道结束。从两端交叉往中间遍历。
    
    
127. 单词接龙

    DFS
    
    int len = Integer.MAX_VALUE;
    
    
        /**
         * DFS 超时
         * @param beginWord
         * @param endWord
         * @param wordList
         * @return
         */
    
        public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    
            if(!wordList.contains(endWord)) return 0;
    
    
            char[][] word_lists = new char[wordList.size()][beginWord.length()];
    
    
            for(int i=0;i<wordList.size();i++){
                word_lists[i] = wordList.get(i).toCharArray();
    
            }
    
    
            dfs(beginWord.toCharArray(),endWord.toCharArray(),word_lists,1);
    
    
            return len == Integer.MAX_VALUE? 0: len;
    
        }
        
        public void dfs(char[] beginWord,char[] endWord,char[][] word_lists,int level){
        
                if(Arrays.equals(beginWord,endWord)) {
                    len = Math.min(len,level);
        
                    return;
                }
        
                for(int i=0;i<word_lists.length;i++){
        
                    int diff = 0;
        
                    char[] temp =  word_lists[i];
        
                    if(temp==null) continue;
        
                    for(int j=0;j<temp.length;j++){
        
                        if(temp[j]!=beginWord[j]) diff+=1;
        
                    }
        
                    if(diff==1){
        
                        word_lists[i] = null;
        
                        dfs(temp,endWord,word_lists,level+1);
        
                        word_lists[i] = temp;
        
                    }
                }
        
            }
            
            
//单向广度优先可通过


//双向广度优先

Set<String> wordSet = new HashSet<String>(wordList);


        int wordLen = beginWord.length();

        wordSet.remove(beginWord);
        if(!wordSet.contains(endWord)) return 0;
        //visited
        HashSet<String> visited = new HashSet<String>();
        //begin queue
        Set<String> startSet = new HashSet<String>(){
            {
                add(beginWord);
            }
        };
        //end queue
        Set<String> endSet = new HashSet<String>(){
            {
                add(endWord);
            }
        };
        int setp =0;
        while(!startSet.isEmpty() && !endSet.isEmpty()){
            ++setp;
            //双方交叉执行
            if(startSet.size()>endSet.size()){
                Set<String> temp = startSet;
                startSet = endSet;
                endSet = temp;
            }
            Set<String> nextLevel = new HashSet<>();
            for(String str:startSet){

                char[] orginalArray = str.toCharArray();

                for(int i =0;i<wordLen;i++){

                    char old = orginalArray[i];

                    for(char k ='a';k<='z';k++){

                        if(k == old){
                            continue;
                        }

                        orginalArray[i] = k;


                        String newStr = new String(orginalArray);

                        if(endSet.contains(newStr)){
                            return setp+1;
                        }


                        if(!visited.contains(newStr) && wordSet.contains(newStr)){

                            visited.add(newStr);

                            //不能直接在加入startSet,那样会永远 startSet > endSet 有可能变成一个单向。所以我们需要用到一个新的 startSet;
                            nextLevel.add(newStr);
                        }

                    }

                    //还原
                    orginalArray[i] = old;
                }
            }
            startSet = nextLevel;
        }

        return 0;
    