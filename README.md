
Introduction
------------

This download agent was part of download-provider in Tizen framework.
It provides basic download functionalities based on libsoup.

It is now separated and can be built into an independent library for external use.

Example
-------

```C
#include <pthread.h>

static GMainLoop* loop = NULL;
static void update_download_info_cb(user_download_info_t *download_info, void *user_param) { }
static void progress_info_cb(user_progress_info_t *progress_info, void *user_param) { }
static void paused_info_cb(user_paused_info_t *paused_info, void *user_param) { }
static void finished_info_cb(user_finished_info_t *finished_info, void *user_param) {
  da_deinit();
  if (g_main_loop_is_running(loop)) {
    g_main_loop_quit(loop);
  }
}

static void* thread_func(void *arg) {
  da_client_cb_t da_cb;
  da_cb.update_dl_info_cb = &update_download_info_cb;
  da_cb.update_progress_info_cb = &progress_info_cb;
  da_cb.finished_info_cb = &finished_info_cb;
  da_cb.paused_info_cb = &paused_info_cb;
  da_init(&da_cb);

  int id;
  da_start_download("http://test.com/sample.mp3", &id);
  loop = g_main_loop_new(NULL, 0);
  g_main_loop_run(loop);
}

int main()
{
  g_type_init();

  pthread_t pt;
  pthread_create(&pt, NULL, &thread_func, NULL);
}
```

For details of usage, please refer to include/download-agent-interface.h
