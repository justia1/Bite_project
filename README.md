# Bite_project

create table CAMPAIGNS (  
  campaign_id     number,  
 campaign_name    varchar2(50) NOT NULL UNIQUE,
  duration number,  
  price number,
  constraint CAMPAIGNS_pk primary key (campaign_id)
  );
  
create table CLIENTS (  
  clients_id     number,  
  phone_num     number NOT NULL UNIQUE,
  balance       number,  
  expiration_date date,
  status varchar2(10) default 'DEACTIVE',
  constraint CLIENTS_pk primary key (clients_id)
  );

