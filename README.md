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

create sequence clients_id_seq nocache;

create or replace trigger clients_id_seq
    before insert or update on CLIENTS
    for each row
begin
    if inserting and :new.clients_id is null then
        :new.clients_id := clients_id_seq.nextval;
    
    end if;
end;

create sequence campaign_id_seq nocache;

create or replace trigger campaign_id_seq
    before insert or update on CAMPAIGNS
    for each row
begin
    if inserting and :new.campaign_id is null then
        :new.campaign_id := campaign_id_seq.nextval;
    end if;
end;

create or replace procedure new_campaign ( p_campaign_name campaigns.campaign_id%type, p_duration number, p_price number) 
IS 
BEGIN 
 
INSERT into campaigns ( campaign_name,  duration,   price ) 
values  
( p_campaign_name,  p_duration,   p_price); 
commit; 
end;

create or replace procedure change_balance (p_clients_id number, p_money number)  
IS  
BEGIN  
  
UPDATE clients  
SET balance = balance+p_money  
where clients_id=p_clients_id;  
UPDATE clients   
set status = (CASE  
        when p_money > 0  
        then 'ACTIVE' 
        else    'DEACTIVE'   
    END) where clients_id=p_clients_id;

    end;  

create or replace procedure order_new_campaign ( p_clients_id clients.clients_id%type, p_campaign_id campaigns.campaign_id%type) 
is kaina number; likutis number;
begin
select price into kaina from campaigns where campaigns.campaign_id= p_campaign_id;
select balance into likutis from clients where clients.clients_id=p_clients_id;
case 
when kaina <= likutis then
change_balance (p_clients_id, -kaina);
insert into client_campaigns(clients_id, campaign_id, expiration_date, price)
    values
        (p_clients_id,
        p_campaign_id,
        (sysdate + (select campaigns.duration  from campaigns where campaigns.campaign_id=p_campaign_id)),
        (select campaigns.price from campaigns  where campaigns.campaign_id=p_campaign_id) );
else DBMS_OUTPUT.PUT_LINE('No sufficient funds');
end case;
end;
