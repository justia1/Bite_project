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

create table CLIENT_CAMPAIGNS ( 
clients_id  number,  
campaign_id number, 
TURN_ON_DATE date DEFAULT (sysdate), 
expiration_date date, 
price number, 
constraint client_campaigns_clid_fk foreign key (clients_id) references  CLIENTS (clients_id) ON DELETE CASCADE, 
constraint client_campaigns_campid_fk foreign key (campaign_id) references  CAMPAIGNS (campaign_id) ON DELETE set NULL 
);
create index client_campaigns_ix on client_campaigns (clients_id, campaign_id);
