##SVN更新操作失败:
###原因:
	Previous operation has not finished; run 'cleanup' if it was interrupted
###解决方法:
	1.  Install sqllite(如果已经安装了SQLite,可以直接看第二步.)
	2.  sqlite中执行sql语句  .svn/wc.db “select * from work_queue”
	3.  sqlite中执行sql语句  .svn/wc.db “delete from work_queue*
###起因:
	 Somehow, svn is stuck on the previous operation.
	 We need to remove this operation from it’s ‘work queue’.