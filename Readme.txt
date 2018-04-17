MySQL SQL查詢語法太複雜(寫太爛)導致系統無法負荷的錯誤訊息 [Fatal error: Maximum execution time of 300 seconds exceeded]

GITHUB: https://github.com/jash-git/MYSQL_Fatal-error-Maximum-execution-time-of-300-seconds-exceeded.git

目前知道是這個原因
	SELECT u.id AS id,u.emp_no AS jobnum,u.security_id AS s_id,u.name AS name, d.name AS dname,u.attribute AS attribute,u.birthday AS birthday,(SELECT COUNT(*) FROM card_for_user_car WHERE card_for_user_car.user_id=u.id) AS card_count FROM user AS u LEFT JOIN department_detail AS d_d ON ((u.id=d_d.user_id) AND ((d_d.car_id IS NULL) OR (d_d.car_id <1))) LEFT JOIN department AS d ON (d_d.dep_id=d.id) ORDER BY u.id;
	Fatal error: Maximum execution time of 300 seconds exceeded in D:\USBWebserver_v8.6\phpmyadmin\libraries\dbi\mysqli.dbi.lib.php on line 267

	SELECT id AS uid,name AS C_name FROM user WHERE id IN (SELECT user_id AS id FROM department_detail WHERE (car_id IS NULL) AND dep_id=0 ORDER BY user_id);
	Fatal error: Maximum execution time of 300 seconds exceeded in D:\USBWebserver_v8.6\phpmyadmin\libraries\dbi\mysqli.dbi.lib.php on line 267

	
解決方法-要調整SQL
	爛的解法:
		SELECT id FROM user ORDER BY id DESC LIMIT 0 , 1;
		SELECT id AS uid,name AS C_name FROM user WHERE (id=單一指定) AND id IN (SELECT user_id AS id FROM department_detail WHERE (car_id IS NULL) AND dep_id=單一指定 ORDER BY user_id);

		SELECT id FROM user ORDER BY id DESC LIMIT 0 , 1;
		SELECT u.id AS id,u.emp_no AS jobnum,u.security_id AS s_id,u.name AS name, d.name AS dname,u.attribute AS attribute,u.birthday AS birthday,(SELECT COUNT(*) FROM card_for_user_car WHERE card_for_user_car.user_id=u.id) AS card_count FROM user AS u LEFT JOIN department_detail AS d_d ON ((u.id=d_d.user_id) AND ((d_d.car_id IS NULL) OR (d_d.car_id <1))) LEFT JOIN department AS d ON (d_d.dep_id=d.id) WHERE u.id=單一指定 ORDER BY u.id;