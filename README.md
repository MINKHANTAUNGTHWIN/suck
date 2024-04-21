# suck

提出物： 　１．ER図 　２．テーブル定義書 　３．作成したSQLスクリプト（例：テーブル作成、データ挿入、クエリ） 　４．クエリの実行結果とそれに対する感想（わかったこと・難しかったところ・工夫したところ　など） １．ER図　ー＞添付画像 ２．テーブル定義書　ー＞SQL文で載せます ３．作成したSQLスクリプト ->テーブル作成はテーブル定義書と同じです store create table public.store ( id_store bigint generated by default as identity, name text not null, constraint store_pkey primary key (id_store) ) tablespace pg_default; orders create table public.orders ( id_orders bigint generated by default as identity, voucher_number timestamp with time zone not null default now(), table_number smallint null, constraint orders_pkey primary key (id_orders) ) tablespace pg_default; anorder create table public.anorder ( id_anorder bigint generated by default as identity, id_orders bigint not null, id_menu bigint null, constraint anorder_pkey primary key (id_anorder) ) tablespace pg_default; menu create table public.menu ( id_menu bigint generated by default as identity, name text not null, price bigint null, constraint menu_pkey primary key (id_menu) ) tablespace pg_default; staff create table public.staff ( id_staff bigint generated by default as identity, name text not null, constraint staff_pkey primary key (id_staff) ) tablespace pg_default; account create table public.account ( id_account bigint generated by default as identity, id_store bigint null, id_staff bigint not null, id_orders bigint null, amount bigint null, date timestamp without time zone null default now(), constraint account_pkey primary key (id_account) ) tablespace pg_default; ３．作成したSQLスクリプト ４．クエリの実行結果とそれに対する感想　ー＞下にクエリと感想を書きます。 SELECT account.id_account, store.name as store_name, staff.name as staff_name, account.id_orders, order_summary.voucher_number, order_summary.table_number, total_price as amount, account.date FROM account LEFT JOIN store ON account.id_store = store.id_store LEFT JOIN staff ON account.id_staff = staff.id_staff JOIN ( SELECT orders.id_orders, orders.voucher_number, orders.table_number, SUM(menu.price) AS total_price FROM orders JOIN anorder ON orders.id_orders = anorder.id_orders JOIN menu ON anorder.id_menu = menu.id_menu GROUP BY orders.id_orders ) AS order_summary ON order_summary.id_orders = account.id_orders; 工夫をしたのはこのクエリです。注文メニューの合計を求めるサブクエリを作って、それをaccount（会計）テーブルにJOINしています。まずは、領収書を発行するのに必要な最小限のデータを、上記クエリで作ってみました。
