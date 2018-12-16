# Bite_project

create table CAMPAIGNS (  
  campaign_id     number,  
 campaign_name    varchar2(50) NOT NULL UNIQUE,
  duration number,  
  price number,
  constraint CAMPAIGNS_pk primary key (campaign_id)
  );
  
