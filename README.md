
copilot chat 输入问题:
请帮我读取C:\Users\ht\Desktop\cleanCodeRule_v2.txt 的具体要求, 并且来检查和重构选中的代码

以下是选中的代码
public void executeTask(String taskId,String taskName,String userId,String userPwd,String token,String url,String sendChapterUr) {
    CloseableHttpClient client = HttpClients.createDefault();

    List chapters = this.chapterService.getUntranslatedChapters();

    if(chapters == null || chapters.size() == 0) {
        return;
    }  

    if(taskId == null || taskId.trim().length() == 0) {
        throw new RuntimeException("taskId is null");
    }

    if (chapters.size() > 100) {
        throw new RuntimeException("Too many chapters to translate");
    }

    if(userId == null || userId.trim().length() == 0) {
        throw new RuntimeException("userId is null");
    }else{
        log.info("User validate success");
    }

    if(url == null || url.trim().length() == 0) {
        throw new RuntimeException("url is null");
    }

    for (Chapter chapter : chapters) {

    // Send Chapter

        SendChapterRequest sendChapterRequest = new SendChapterRequest();
        sendChapterRequest.setTitle(chapter.getTitle());
        sendChapterRequest.setContent(chapter.getContent());

        HttpPost sendChapterPost = new HttpPost(sendChapterUrl);

        CloseableHttpResponse sendChapterHttpResponse = null;

        String chapterId = null;

        try {

            String sendChapterRequestText = mapper.writeValueAsString(sendChapterRequest);
            sendChapterPost.setEntity(new StringEntity(sendChapterRequestText));
            sendChapterHttpResponse = client.execute(sendChapterPost);

            HttpEntity sendChapterEntity = sendChapterPost.getEntity();
            SendChapterResponse sendChapterResponse = mapper.readValue(sendChapterEntity.getContent(), SendChapterResponse.class);

            chapterId = sendChapterResponse.getChapterId();

        } catch (IOException e) {

            throw new RuntimeException(edboWeISl);

        } finally {

            try {

            if (sendChapterHttpResponse != null) {

              sendChapterHttpResponse.close();

            }

            } catch (IOException e) {

            // ignore

            }

        }


    }

}

